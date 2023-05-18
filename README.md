# # Project1: Game Theory
## Project information
- **Author**: Ni(Jenny) Zheng, Computational Design, Social Policy track, Class of 2026, Duke Kunshan University
- **Instructor**: Prof. Luyao Zhang, Duke Kunshan University
- **Disclaimer**: Submissions to the Problem Set No. or Final Project for [COMPSCI/ECON 206 Computational Microeconomics, 2023 Spring (Seven Week - Second)](https://ce.pubpub.org/) instructed by Prof. Luyao Zhang at Duke Kunshan University.
- **Acknowledgments**: Prof. Luyao Zhang
[notes: please include all professors, students, and staff who have contributed to your completetion of the project.]
- **Project Summary**: 
  - [Summarize the Background/Motivation]
  - [Research Questions]
  - [Application Scenario]
  - [Methodology]
  - [Results]
  - [Intellectual Merits and Practical impacts of your project.]
  
   
Note: please insert the screenshot of the answers to your research question by ChatGPT. The methodology that you use to address the research questions must be more innovative than both the current literature and ChatGPT. 

## Table of Contents

- model
- code
- spotlight
- more about the author
- references

### Model
- [Game Environment]

There are two players: the "investor" (A) and the "trustee"(B). The game starts with A.
A is given an endowment and decides how much of that money to invest B. However the investment from A is, it will be tripled.
(eg. If A got $5, A can choose any amount between 0 and 15 to invest B. If A chooses to invest $5, B receives $15.)
B then decides how much of that money to return to the investor. B can choose to return any amount between $0 and $15.

The game is repeated for several rounds, with the roles of investor and trustee switching between the two players. Both of them can negotiate with each other, but any of their promise in the negotiation may not actually work.

This game is largely depend on the trust in each other and whether A and B choose to collaborate or not. The investor must decide how much to trust the trustee, and the trustee must decide how much to repay the investor's trust, where we should always take facts of social behavior into consideration in the real world (Cesarini et al. 2008).

- [Solution Concept]

The concept of a solution for this trust game involves exploring the strategies and outcomes that may arise between the investor (Player A) and the trustee (Player B) while aiming for Pareto efficiency. Pareto efficiency is a widely used efficiency concept in game theory that focuses on maximizing the total benefit without making any player worse off (Stiglitz 1982).

One possible solution is a win-win cooperation strategy, where both A and B agree to share the total benefit equally. This strategy fosters cooperation, trust, and fairness. By splitting the total benefit equally, both players achieve an outcome that is Pareto efficient, as no player can be made better off without making the other player worse off.

Another possible solution is a gamble strategy, where one player (either A or B) decides to go all-in with their money, taking a high-risk, high-reward approach. This strategy can lead to Pareto efficient outcomes if both players agree to share the resulting benefit equally. However, it is important to note that the gamble strategy may introduce risks and potential losses, which can undermine the fairness and stability of the game.

In situations where trust is low, a risk minimization strategy may be adopted. Both A and B aim to minimize their risk by investing smaller amounts. While this strategy may not maximize the total benefit, it allows both players to limit their potential losses and maintain a sense of fairness. The risk minimization strategy can lead to Pareto efficient outcomes when both players have a similar level of risk aversion (Chin 2009).

It is crucial to emphasize that achieving Pareto efficiency does not guarantee fairness in terms of wealth distribution. The trust game inherently involves asymmetry, as one player takes on the role of an investor while the other acts as a trustee. However, by focusing on Pareto efficiency, the solution concept ensures that both players maximize their overall benefits without worsening each other's outcomes (Rasti, Sharafat, and Seyfe 2009).

Overall, the solution concept of the trust game incorporates strategies that aim to achieve Pareto efficiency while considering trust, risk, and fairness. By analyzing the game from this perspective, players can make informed decisions that optimize their individual benefits while maintaining a cooperative and mutually beneficial relationship.

- [Strategies]

- Player A's Strategies:
  **High Trust Strategy:** A chooses to invest a high amount in B, indicating a high level of trust. A believes that B will return the investment with interest.
  *Action:* A selects a high amount to invest in B.
  *Contingency:* A expects B to reciprocate by returning a higher amount.

  **Low Trust Strategy:** A chooses to invest a low amount in B, indicating a low level of trust or a desire to avoid high risks.
  *Action:* A selects a low amount to invest in B.
  *Contingency:* A expects B to reciprocate by returning a lower amount.
  
- Player B's Strategies:
  **High Return Strategy:** B chooses to return a higher amount to A, aiming to maintain trust and foster a cooperative relationship.
  *Action:* B selects a higher amount to return to A.
  *Contingency:* B expects A to continue investing a high amount in future rounds.

  **Low Return Strategy:** B chooses to return a lower amount to A, potentially leading to a breakdown in trust and cooperation.
  *Action:* B selects a lower amount to return to A.
  *Contingency:* B expects A to invest a lower amount in future rounds or to adopt a more risk-averse strategy.
  
- Both Players' Strategies:
  **Win-Win Cooperation Strategy:** Both A and B agree to share the total benefit equally, fostering cooperation and mutual trust.
  *Action:* Both players agree to split the total benefit equally.
  *Contingency:* Both players receive the same sum of money, leading to a fair and cooperative outcome.
  
  **Gamble Strategy:** Either A or B decides to go all-in with their money, taking a high-risk, high-reward approach.
  *Action:* One player (either A or B) invests their entire amount, while the other player receives the investment.
  *Contingency:* The outcome depends on the actions of the player who receives the investment, which can result in either a significant loss or a substantial gain for both players.
  
  **Risk Minimization Strategy:** Both A and B aim to minimize their risk by investing smaller amounts, particularly when trust is low.
  *Action:* Both players invest a small fraction of their total amount.
  *Contingency:* Both players limit their potential losses in case the other player does not reciprocate.

The players can choose different strategies based on their individual preferences, risk tolerance, and beliefs about the other player's behavior.

- Evaluations
- Fairness:
The game is always unfair for both A and B.
1)Risk: The tripled investment from A may involve a significant amount of risk for B, as they are not guaranteed to receive any return on the investment. This may lead to a situation where the trustee feels hesitant to invest the money they receive, as they do not want to risk losing it.

2)Trust: The game relies heavily on trust between the two players, as the A must trust that B will return some portion of the investment, and B must trust that the investor will not invest an unfairly low amount. If either player lacks trust in the other, this may lead to a breakdown in cooperation and a suboptimal outcome for both players.

3)Negotiation: The fact that promises made during negotiation may not be kept adds another uncertainty to the game. If either player feels that they have been misled or taken advantage of during negotiation, this may lead to a breakdown in trust and cooperation.


##  Code
Game Tree:

    from otree.api import *


    doc = """
    Simple trust game
    """


    class C(BaseConstants):
        NAME_IN_URL = 'trust_simple'
        PLAYERS_PER_GROUP = 2
        NUM_ROUNDS = 1
        ENDOWMENT = cu(10)
        MULTIPLIER = 3


    class Subsession(BaseSubsession):
        pass


    class Group(BaseGroup):
        sent_amount = models.CurrencyField(
            min=cu(0),
            max=C.ENDOWMENT,
            doc="""Amount sent by P1""",
            label="How much do you want to send to participant B?",
        )
        sent_back_amount = models.CurrencyField(
            doc="""Amount sent back by P2""", label="How much do you want to send back?"
        )


    class Player(BasePlayer):
        pass


# Functions
    def sent_back_amount_choices(group: Group):
        return currency_range(0, group.sent_amount * C.MULTIPLIER, 1)


    def set_payoffs(group: Group):
        p1 = group.get_player_by_id(1)
        p2 = group.get_player_by_id(2)
        p1.payoff = C.ENDOWMENT - group.sent_amount + group.sent_back_amount
        p2.payoff = group.sent_amount * C.MULTIPLIER - group.sent_back_amount


# Pages
    class Send(Page):
    form_model = 'group'
    form_fields = ['sent_amount']

    @staticmethod
    def is_displayed(player: Player):
        return player.id_in_group == 1


        class WaitForP1(WaitPage):
        pass


    class SendBack(Page):
    form_model = 'group'
    form_fields = ['sent_back_amount']

    @staticmethod
    def is_displayed(player: Player):
        return player.id_in_group == 2

    @staticmethod
    def vars_for_template(player: Player):
        group = player.group

        return dict(tripled_amount=group.sent_amount * C.MULTIPLIER)


        class ResultsWaitPage(WaitPage):
        after_all_players_arrive = set_payoffs


        class Results(Page):
        pass


     page_sequence = [Send, WaitForP1, SendBack, ResultsWaitPage, Results]

  **Description**
  The code is an implementation of a simple trust game using the oTree framework. The trust game involves two players: player 1 and player 2, showing how these players make decisions and their payoffs are calculated based on their choices.

- `Group` represents a group of players. It contains fields to store the amount sent by player 1 (`sent_amount`) and the amount sent back by player 2 (`sent_back_amount`).
- `sent_back_amount_choices` determines the choices for the amount sent back by player 2 based on the amount sent from player 1.
- `set_payoffs` calculates the payoffs for each player in the group based on the amount sent and sent back.
- `ResultsWaitPage`, and `Results` are used to calculate and display the payoffs after all players have made their decisions.

### Spotlight
- Review articles

1."Behavioral economics: Reunifying psychology and economics" is a book that at the intersection of psychology and economics.
The book introduces basic concepts of behavioral economics, showing that individuals are often irrational and make decisions based on emotions and biases, various biases and heuristics that influence decision-making in individuals, and using the approach of behavioral economics to policy-making.
  -Challenge to trust game
According to traditional game theory, the rational choice for both players is to maximize their own payoffs, which would lead the trustor to send no money and the trustee to keep all the money received. However, in fact, trustors often return some or all of the tripled amount. Variables are hard to predicted precisely. It challenges the assumptions of rational choice theory and suggest that humans are not purely self-interested, but also care about fairness, reciprocity, and social norms. The authors suggest that incorporating these factors into economic models can lead to a more accurate understanding of human behavior and more effective policy solutions.

2."Incorporating Fairness into Game Theory and Economics" provides a theoretical framework for incorporating fairness into game theory, and empirical evidence to support the theory. Rabin introduces the concept of "other-regarding preferences", which refers to the idea that individuals care not only about their own payoffs but also about the payoffs of others. He argues that these preferences are an important factor in many economic interactions, and that they can be incorporated into game theory models by using the concept of social utility functions. Besides,Rabin also introduces the concept of "equity-efficiency tradeoffs", which refers to the idea that fairness concerns can sometimes lead to outcomes that are less efficient from an economic perspective. He argues that these tradeoffs are an important consideration in policy-making, and that a better understanding of fairness can lead to more effective policy solutions.

-More research questions in the real world?

In the real world, we should consider social behaviors of both players, which means that they will be affected by many other factors when continuing/stopping their game.

1) Is there an agency between two players? If one person wants to get higher amount of money than the initial endowment but there is no best player B to help him, agency very likely works to match the best couple of players with similar needs or strategies.

2) Does extra cost exist in the game? How they negotiate with each other?

3) When does the game end?
-when there is the huge gap?
-when one person thinks he has got enough money and wants to exit the game?
-when their profit maximized?

4) What are people's expectation of this game?
-maximized profit?
-gain some money with little risk of investment?
-huge gap beyond others in wealth?

### More about the Author
- headshot
![_DSC1207](https://user-images.githubusercontent.com/125801773/230779922-be3c5551-9678-4dc3-a896-22ebfddfa6a7.JPG)

- Self-introduction

  Hi, I am Ni(Jenny) Zheng from Shanghai. My intended major is Computational Design with track in Social Policy. In the future, I'd like to dig deeper into computational social science (economics/environmental science) and do some quantitative research.
### References

- Literature References in [Chicago Author-Date](https://www.chicagomanualofstyle.org/tools_citationguide/citation-guide-2.html) Style and [BibTex](https://scholar.google.com/) 

Camerer, Colin. 1999. ‘Behavioral Economics: Reunifying Psychology and Economics’. Proceedings of the National Academy of Sciences 96 (19): 10575–77. https://doi.org/10.1073/pnas.96.19.10575.

Rabin, Matthew. 1993. ‘Incorporating Fairness into Game Theory and Economics’. The American Economic Review 83 (5): 1281–1302.

Levin, Dan, and Luyao Zhang. 2020. “Bridging Level-K to Nash Equilibrium.” *The Review of Economics and Statistics* 104 (6): 1329–40. https://doi.org/10.1162/rest_a_00990.

```
@article{camerer1999behavioral,
  title={Behavioral economics: Reunifying psychology and economics},
  author={Camerer, Colin F},
  journal={Proceedings of the National Academy of Sciences},
  volume={96},
  number={19},
  pages={10575--10577},
  year={1999},
  publisher={National Academy of Sciences}
 
}

@article{rabin1993incorporating,
  title={Incorporating fairness into game theory and economics},
  author={Rabin, Matthew},
  journal={The American Economic Review},
  volume={83},
  number={5},
  pages={1281--1302},
  year={1993},
}

@article{levin2022bridging,
  title={Bridging level-k to nash equilibrium},
  author={Levin, Dan and Zhang, Luyao},
  journal={Review of Economics and Statistics},
  volume={104},
  number={6},
  pages={1329--1340},
  year={2022},
  publisher={MIT Press One Rogers Street, Cambridge, MA 02142-1209, USA journals-info~…}
}
```

