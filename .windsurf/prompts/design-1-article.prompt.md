# Design 1 article

Write an article titled "Fractalgram design principles. Part 1".

## Context

* [Definition of respect game](../../concepts/respect-game.md)
* [Definition of fractalgram](../../concepts/fractalgram.md)

Can use fragments of this text I started earlier: [design-1-article.ctx.md](./design-1-article.ctx.md)

## Article outline

- Introduction

- How the original game introduced in fractally whitepaper worked
    - People use any process they want to reach consensus
    * The system just listens to the final consensus signal from participants - it distributes respect if there's consensus. If there's no consensus no one gets respect;
    - System does not give a voting tool that determines who gets respect and it does not prescribe how to build consensus 
    - People use discussion and negotiation to arrive at consensus
    
Relevant quote from fractally whitepaper:
>The group can use any process they like to reach consensus so long as everyone agrees by
the end of the 1 hour window. ƒractally intentionally avoided implementing a voting and
tallying system because all such systems encourage people to “vote strategically” instead of
honestly. Instead the meeting should be a back and forth discussion and negotiation.
The lack of a “voting” system means that people are forced to build trust that everyone is in
agreement so that they can accurately report their opinion on the consensus. Building trust
is a vital part of strong communities and identifying those who violate trust is critical to
securing the integrity of a community.


- What makes the game great;
    * Subjectivity
        * Gall's law
    - Listening - everyone gets the chance to speak (present their contributions) and people have to listen, because they will have to reach consensus;
    * Accuracy and fairness through dialectic; 
    * Building trust, connection and understanding through consensus building
        - Space to express any sense of unfairness (people can advocate for themselves in respect game)
    * Practicing consensus building
        - Incentive - if we don't have consensus, no one gets respect
        - Key skill in today's world
    - Creates a space to detect bad behaviour like cartels, selfish voting (not self-voting, but selfish voting);

- Note that at least 4 of 6 of the above points depend on the need to build consensus (you don't get these benefits if you remove the need to build consensus (e.g.: if you create a voting system that determines results from initial votes of people))
    - *Listening* - if there's no need to build consensus there's less incentive to listen. People can just have strategy prepared for how to vote for themselves and their friends regardless of what others present. No situation where they would have to share their opinion, present arguments for their position;
    * *Accuracy and fairness* - this is the effect of consensus building. By synthesizing different perspectives we can approach the truth;
    * *Building trust and connection*
    - *Practicing consensus building*

- How implementations of Respect game deviate from what makes this game great
    - Apps implementing respect game in a way where consensus building is replaced by a voting system that determines rankings from initial votes of participants;
    - Apps implementing respect game that assumes certain voting thresholds mean consensus. 
    - Or even if we use an app that allows prioritizes consensus building, we create a voting system ourselves (e.g.: apply a rule 4/6 votes means consensus).
        - If the goal is consensus then is a helpful heuristic we should not forget that it is only that. If someone has a good argument to vote otherwise, he should (ideally) get space to present his argument and try to swing votes to another direction. **If the goal is consensus then initial votes in fractalgram just express everyone's initial opinions. It's just the start of consensus building.**
            - We have to leave the space for the minority opinion to express itself in respect game. Not doing this creates a space for build up of bad emotion like resentment;
    - Anonymous voting;
        - People have to hide from social responsibility of their votes. This leads to them not having to listen, etc.
    - We rarely share perspectives and discuss;

- Main takeaways
    - Building consensus is hard but we have to thank it for many of the benefits of respect game;
    - **Respect game process should be totally human controlled. The role of the app is just to help people communicate in building of consensus;**
        - **The app should not tell the group when consensus is reached. People should determine that themselves;**

- Challenges
    - How should the app determine when to proceed to the next step?
        - It could require 100% consensus.
            - But this is not realistic in the real world - network connection issues, people being away from keyboard (or being otherwise distracted in the real world), mean that sometimes (from my experience quite often) 100% consensus is impossible and not because someone actively disagrees;
        - **A single moderator / host who controls the process is the solution;**
    - Do we have to trust the host of the room then? Isn't this un-democratic?
        - We make forking of the room easy
            - Make creation of new respect game rooms easy. Then if participants of the room are unhappy with how the host controls the process, any single one of them can create a new room with a better.
        - Note: this won't work if in your fractal system creation of breakout rooms is couples to respect distribution mechanism. I mean if your smart contracts create breakout-rooms that can earn respect, then people cannot easily create new breakout room sessions. Unless you create some other method for people to for breakout rooms. But I think a lot simpler solution is to do what ORDAO fractals do - ORDAO (as executive branch) has its own consensus process that people can use to execute results from respect game, regardless of how the rooms are created and managed. ORDAO listens to the final consensus from respect game and is agnostic to how this consensus was built. ^w84z95

- Summary / conclusion
    - A fractalgram session has to be controlled by a human moderator.
    - It is a better solution even if we prefer some kind of threshold of votes on an option to determine when the consensus is reached - unreliable nature of online game like this means we can't consistently depend on voting thresholds like this;

