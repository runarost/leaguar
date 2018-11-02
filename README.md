# League Guarantee
Finding an algorithm for guaranteed upper and lower positions of a team in a league given the current standings and remaining matches.
Additionally tools for simulating leagues.

# Motivation
When making a table visualization, it's always cool to determine if someone has secured promotion/relegation/European cup play/etc. Often it's easy.

As an example, say we have a league where two teams get promoted, and the points for win/draw/loss is 3/1/0 points. There's one match left for every team. Team A has a 2 point lead on Team B and Team C. In this situation it's reasonable to make a simple mathematical calculation that Team A can lose the final match, and Team B and Team C win their final matches and pass Team A on the table. This means Team A still hasn't been secured promotion.

But the complicating factor is that teams can play each other. If for example the final matches is Team A vs. Team X, and Team B vs. Team C. If Team A loses, then both Team B and Team C can pass Team A, *but not both teams at the same time*.

In order to make an absolute guarantee, simple arithmetics isn't enough, you need to simulate the remaining matches. However, simulating N remaining matches with M outcomes means you ned to do M to the power of N. This is not feasible for many remaining matches. Especially if the list of outcomes per match (like in best of 11 legs in darts) is big.

Very likely the only way to do this with a common algorithm for all combinations of current standings and remaining matches (both when there are just a few remaining matches and when there are many remaining matches) to have strategies for reducing the number of matchresult combinations that you need to simulate.

# Simulation ideas
* Make a process that takes as parameters a league standing, results of remaining matches, and has some data as output (league)
* Make a process that iteratively or recursively makes exhaustive list of result outcomes (but it's n matches to the power of m outcomes, so need to be able to not create everything up front)
* Make simulations that give both upper/lower guaranteed positions, and likely positions
* Make something that gives probabilities of outcomes based on some input (prematch average odds, ELO rating)
* Make a simulation that takes all possible match results, with equal weighting to each result
* Make a simulation that takes a monte carlo approach, with random results
* Make a simulation that takes all possible match results, but with weighting based on probabilities
* Make a simulation that takes a monte carlo approach, with weighted results

# Guarantee ideas
* An easy way to make a upper bound is to consider for a player that he wins all remaining matches with best score and all opponents individually lose all their matches with the worst score. A player cannot place higher than that. However, there can potentially be stricter upper bounds (since all opponents can't lose all remaining matches if they play at least one match against each other).
* An easy way to make a lower bound is to consider for a player that he loses all remaining matches with worst score and all opponents individually win all their matches with the worst score. A player cannot place lower than that. However, there can potentially be stricter lower bounds (since all opponents can't win all remaining matches if they play at least one match against each other).
* With many remaining matches, this will likely be **the** correct upper/lower guarantee. But there's no way for sure to figure this out unless everything is simulated.
* Perhaps it's simpler to only consider team by team.
* And stop considering the team if there is two outcomes where the final top/bottom position matches the upper/lower bound found mathematically.
* Could just trying a monte carlo approach for X seconds be a strategy to see if randomness quickly confirms upper/lower bounds as actual possible final standings and then stop?
* Could exhaustive combinations for all matches given number of matchresult combinations being lower than X be an approach to cover simulations with few remaining matchresults?
* Could a smart thing be to consider team by team, and then reducing the number of *interesting* matches by applying logic, as described in [this resource](https://stackoverflow.com/questions/16538952/algorithm-to-determine-the-highest-and-lowest-possible-finishing-position-of-a-t)?