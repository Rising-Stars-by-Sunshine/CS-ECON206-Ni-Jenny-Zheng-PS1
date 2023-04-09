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
- Game Environment

There are two players: the "investor" (A) and the "trustee"(B). The game starts with A.
A is given an endowment and decides how much of that money to invest B. However the investment from A is, it will be tripled.
(eg. If A got $5, A can choose any amount between 0 and 15 to invest B. If A chooses to invest $5, B receives $15.)
B then decides how much of that money to return to the investor. B can choose to return any amount between $0 and $15.

The game is repeated for several rounds, with the roles of investor and trustee switching between the two players. Both of them can negotiate with each other, but any of their promise in the negotiation may not actually work.

This game is largely depend on the trust in each other and whether A and B choose to collaborate or not. The investor must decide how much to trust the trustee, and the trustee must decide how much to repay the investor's trust, where we should always take facts of social behavior into consideration in the real world.

- Solution Concept

The concept of solution for this trust game involves exploring the strategies and outcomes that may arise between the investor and the trustee.
One possible solution is that A chooses to invest a high amount in the trustee, indicating a high level of trust in them to return the investment with interest. One of the motivations of A may be desire for maximized profit because if all of A's money is tripled, A has higher probabilities to get more money than A's original sum of money as long as long A and B reach an agreement that they will share the total benefit (not necessarily equally).
In response, the trustee may return a higher amount to the investor to maintain the trust and avoid any potential negative consequences of breaking the trust. Also, since B doesn't have money in the very beginning, no matter how A invest to B and whether B return to A, B will not lose any actual benefit. If B wants to maximize the total benefit, B may raise the win-win cooperation, which means share the average of total income of both. This, in turn, may increase the level of trust between the two players and lead to a more cooperative and profitable relationship in future rounds.
Another possible solution is that A chooses to invest a low amount in B, indicating a low level of trust or a avoiding high risks of potential losses. In response, the trustee may be lees likely tod return a high amount to the investor, potentially leading to a breakdown in trust and cooperation in future rounds. Additionally, the potential for broken promises and negotiation may create uncertainty and unpredictability in the outcomes of the game.

- Strategies

1. win-win cooperation: Under this circumstance, the sum of investment isn't important because whatever they invest or pay back is based on the win-win cooperation that all of their money should be split equally. The most important foundation of their goal of win-win is their long-term trust and willingness to continue the cooperation.

- Payoffs: A and B share the total benefit equally. After several rounds, both A and B will get the same sum of money, which is the way that is rather equal and most beneficial to both of players.

2. gamble: all in the money and face the risk of high risk of entire loss or huge sum of benefit. Both A and B can be the gambler, and they can conduct in any round.

- Payoffs: If A has sufficient trust in B, or have enough confidence that A oneself will get the largest interest from the return of A's investment, or pursue the efficiency of their trading-off and wants to get large amount of money as soon as possible, A will be the gambler and his fortune will totally depend on the other player. For B who receive the all-in money from A, B has the chance to betray A, or follow A's hope with some other costs.

3. risk minimization: very small amount of investment each time, and it may iteratively increase as the strengthen of the trust in each other.
- Payoffs: Both A and B will highlight the risk they run so that they may invest less than 50% of their total sum of money based on their evaluation on others if the opposite doesn't pay back. In the situation, both players don't have much trust in each other, and they only care the potential of their loss.

- Evaluations

- Fairness:
The game is always unfair for both A and B.
1)Risk: The tripled investment from A may involve a significant amount of risk for B, as they are not guaranteed to receive any return on the investment. This may lead to a situation where the trustee feels hesitant to invest the money they receive, as they do not want to risk losing it.

2)Trust: The game relies heavily on trust between the two players, as the A must trust that B will return some portion of the investment, and B must trust that the investor will not invest an unfairly low amount. If either player lacks trust in the other, this may lead to a breakdown in cooperation and a suboptimal outcome for both players.

3)Negotiation: The fact that promises made during negotiation may not be kept adds another uncertainty to the game. If either player feels that they have been misled or taken advantage of during negotiation, this may lead to a breakdown in trust and cooperation.

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

##  Code
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


### Spotlight
- Posters
- Figures
- Slides
- Presentations
- Review articles
- Media appearance

### More about the Author
- headshot
![_DSC1207](https://user-images.githubusercontent.com/125801773/230779922-be3c5551-9678-4dc3-a896-22ebfddfa6a7.JPG)

- Self-introduction
  Hi, I am Ni(Jenny) Zheng from Shanghai. My intended major is Computational Design with track in Social Policy. In the future, I'd like to dig deeper into computational social science (economics/environmental science) and do some quantitative research.
### References

- Literature References in [Chicago Author-Date](https://www.chicagomanualofstyle.org/tools_citationguide/citation-guide-2.html) Style and [BibTex](https://scholar.google.com/) 

Levin, Dan, and Luyao Zhang. 2020. “Bridging Level-K to Nash Equilibrium.” *The Review of Economics and Statistics* 104 (6): 1329–40. https://doi.org/10.1162/rest_a_00990.

```
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

