**A Reinforcement Learning Based Approach for Optimization of
Hydropower’s “Dual Mandate” in Equitable Water Management
and Profitable Power Generation and Storage**

**Project Background**

At the beginning of the semester, Professor Seungjin Whang and I took on an ambitious project to explore the relevance of sub-seasonal weather forecasting, an area popularized for predictive modeling by the Subseasonal Rodeo Project to optimization modeling.  In particular, the journey was meant to allow me to explore a variety of Reinforcement Learning methods through Udacity’s “Deep Reinforcement Learning” offering and ultimately put together a real-world implementation in the context of climate forecasting.  

Mid-way through the quarter, as we began to search for a relevant project area, Professor Erica Plambeck and Professor Gretchen Daily, played a key role in helping me navigate to the domain of hydropower.  Hydropower is well suited to dynamic modeling in that it has near vs. long-term objective functions, often is subject to the control of a variety of different stakeholders, and, at times, is subject to external market dynamics.  Hydropower plants often face difficult tradeoffs between maintaining hydrological dynamics both up and downstream and generating power as needed for relevant grids.  At the same time, upstream and downstream power plants are often managed by different state and local jurisdiction.  And lastly, the nature of power is that while many hydropower plants choose to sell electricity at a fixed rate today, most power generation is sold in a more dynamic pricing environment. 

As I learned throughout the quarter, these sorts of conditions are often optimal for Reinforcement Learning.  Reinforcement learning seeks to use high-frequency data or, often, simulated data events to “learn” effective policies for managing control situations.  By learning to predict future rewards from a given action at a given point in time, these systems can evolve rules and procedures more dynamically than rules based systems and at a higher dimensionality than many other optimization methods.  

To put these methods into practice, I worked to develop a simplified scenario that modeled the Colorado River system.  The Colorado River Basin extends from Wyoming through to Baja California Mexico and plays a major role in the electricity supply of Utah, Nevada, Arizona, and California.  Along the river, there are four major power plants, the Glen Canyon Dam, the Hoover Dam, the Davis Dam, and the Parker Dam.  Among these, the Hoover Dam is particularly interesting because it both generates electricity and has the capacity to pump excess power capacity upstream to serve as “at scale” storage for the grid.  

![image](https://user-images.githubusercontent.com/58300517/83983832-cdffd600-a8ee-11ea-858d-7c86837c7c7b.png)

The system, as outlined, used sub-seasonal and daily precipitation data from dependent communities accessed via NOAA to model a Reinforcement Learning based control system for the Hoover Dam.  

**Methods**

The modeling methodology for this project can best be described in four parts – Data Resampling Methods, Hydrology Modeling, Power Generation Modeling, and Learning Algorithm Design.  The details of this implementation are best visualized below.

![image](https://user-images.githubusercontent.com/58300517/83983847-e243d300-a8ee-11ea-9dd7-b3be09d061a7.png)

Key to all outcomes in reinforcement learning is the Resampling Method used to simulate the control environment.  For the purposes of this project, I looked at 5 years of historical data from 2015-2019 due to significant changes in precipitation and hydrological conditions of the Colorado River prior to that time period.  Specifically, we used Parametric Bootstrapping, randomly sampling precipitation data from within 10 days any of the date being projected for the given year, leaving a total of 20x5=100 possible options for each day of each 5-year period.  While data was modeled daily, analysis was primarily conducted on a quarterly basis to control for uncertainty and limit computational requirements. 

With a given “season” or quarter modeled, the hydrology relevant to the Hoover Dam was modeled based on upstream precipitation in the communities of Colorado Springs CO, St. George UT, Flagstaff AZ, as well as the nearby communities of Las Vegas NV and Kingman AZ.  Precipitation in each of these areas was indexed relative to the maximum precipitation experienced in that area and then a simple policy was used to determine the generation of up and downstream plant operations as well as tributary inflows.  As a result, a reward could be established for remaining within +/- 5% of water levels in Lake Mead, Hoover’s upstream reservoir.  

The actions of these plants also had an impact on Hoover.  While the learned policy was ultimately responsible for Hoover’s behavior, a dynamic price table which used the other dams’ behavior as a proxy for demand/price was put in place.  This price table, when combined with action selected by the policy, ultimately determined Hoover revenues.  Consequently a reward could be established for actions that led to revenue in excess of a typical quarter for the Hoover power plant.  The combination of this and the hydrological model meant that the algorithm ultimately searched for a policy that would increase revenues without jeopardizing the river basin’s hydrology on aggregate.  

Lastly the learning algorithm implemented for this task was a variation on SARSA MAX or Modified Connectionist Q-Learning.  The nature of the bootstrapping meant that this approach was essentially a variant on Monte Carlo control where data was sampled daily and in 5-year increments, but the policy was updated quarterly.  In order to ensure thorough search of the full learning space, we implemented a linear decay of the epsilon greedy policy from 1 to 0 over the live of the event as well as a linear decay of the learning rate from .1 to .001.  The predicted value for each reward was also discounted with beta=.02.  

**Results**

Due to limited compute capacity, this method was only implemented for N=2500 bootstrapped samples or 50,000 policy changes and ~4.57M events.  While this is a reasonable number of samples, given a Q-Table with dimensions ~6*6500 and therefore ~39,000 parameters to update convergence was not guaranteed.  

These results, as shown below, do represent a positive trend towards revenues in excess of 100% of quarterly averages for the Hoover Dam.  While much more work would need to be done before putting these results in practice, these early results suggest that there could be merit to a hydrologically balanced but profit maximizing dynamic-policy based approach to running the dam.


![image](https://user-images.githubusercontent.com/58300517/83983858-f8ea2a00-a8ee-11ea-82b9-4f0959ebb597.png)

**Learnings**

Ultimately this 390 was incredibly rewarding and I couldn’t be more thankful to Professor Whang for taking this project on with me.  Not only did I gain significant skill through the Udacity exercises, but I greatly challenged myself by executing a final project using only low-level libraries.  As a result, I learned many of the “tricks” to building practical implementations. 

More concretely, I learned that while the allure of Reinforcement Learning systems is it’s ability to handle high levels of dimensionality, simplifying assumptions remain a necessity.  While these assumptions can feel unnatural, they still represent a modeling scenario far more realistic than prior optimization methods.  

I’m confident that by keeping these lessons and the raw skills I learned in mind, I’ll be a better business person and data scientist for many years to come.  


