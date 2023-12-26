# Stock-Trading-Gym-Apple-Data-
AnyTrading is a collection of OpenAI Gym environments for reinforcement learning-based trading algorithms.

Trading algorithms are mostly implemented in two markets: FOREX and Stock. AnyTrading aims to provide some Gym environments to improve and facilitate the procedure of developing and testing RL-based algorithms in this area. This purpose is obtained by implementing three Gym environments: TradingEnv, ForexEnv, and StocksEnv.

TradingEnv is an abstract environment which is defined to support all kinds of trading environments. ForexEnv and StocksEnv are simply two environments that inherit and extend TradingEnv. 
![Uploading Ekran Resmi 2023-12-26 23.52.02.png…]()

**Trading Actions**
If you search on the Internet for trading algorithms, you will find them using numerous actions such as Buy, Sell, Hold, Enter, Exit, etc. Referring to the first statement of this section, a typical RL agent can only solve a part of the main problem in this area. If you work in trading markets you will learn that deciding whether to hold, enter, or exit a pair (in FOREX) or stock (in Stocks) is a statistical decision depending on many parameters such as your budget, pairs or stocks you trade, your money distribution policy in multiple markets, etc. It's a massive burden for an RL agent to consider all these parameters and may take years to develop such an agent! In this case, you certainly will not use this environment but you will extend your own.

So after months of work, I finally found out that these actions just make things complicated with no real positive impact. In fact, they just increase the learning time and an action like Hold will be barely used by a well-trained agent because it doesn't want to miss a single penny. Therefore there is no need to have such numerous actions and only Sell=0 and Buy=1 actions are adequate to train an agent just as well.

**Trading Positions**
In a simple vision: Long position wants to buy shares when prices are low and profit by sticking with them while their value is going up, and Short position wants to sell shares with high value and use this value to buy shares at a lower value, keeping the difference as profit.

Again, in some trading algorithms, you may find numerous positions such as Short, Long, Flat, etc. As discussed earlier, I use only Short=0 and Long=1 positions.

**TradingEnv**
TradingEnv is an abstract class which inherits gym.Env. This class aims to provide a general-purpose environment for all kinds of trading markets. 

**Properties:**

- df: An abbreviation for DataFrame. It's a pandas' DataFrame which contains your dataset and is passed in the class' constructor.

- prices: Real prices over time. Used to calculate profit and render the environment.

- signal_features: Extracted features over time. Used to create Gym observations.

- window_size: Number of ticks (current and previous ticks) returned as a Gym observation. It is passed in the class' constructor.

- action_space: The Gym action_space property. Containing discrete values of 0=Sell and 1=Buy.

- observation_space: The Gym observation_space property. Each observation is a window on signal_features from index current_tick - window_size + 1 to current_tick. So _start_tick of the environment would be equal to window_size. In addition, initial value for _last_trade_tick is window_size - 1 .

- shape: Shape of a single observation.

- history: Stores the information of all steps.

**Methods:**
- seed: Typical Gym seed method.

- reset: Typical Gym reset method.

- step: Typical Gym step method.

- render: Typical Gym render method. Renders the information of the environment's current tick.

- render_all: Renders the whole environment.

- close: Typical Gym close method.

**Abstract Methods:**
- _process_data: It is called in the constructor and returns prices and signal_features as a tuple. In different trading markets, different features need to be obtained. So this method enables our TradingEnv to be a general-purpose environment and specific features can be returned for specific environments such as FOREX, Stocks, etc.

- _calculate_reward: The reward function for the RL agent.

- _update_profit: Calculates and updates total profit which the RL agent has achieved so far. Profit indicates the amount of units of currency you have achieved by starting with 1.0 unit (Profit = FinalMoney / StartingMoney).

- max_possible_profit: The maximum possible profit that an RL agent can obtain regardless of trade fees.


