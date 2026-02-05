# Fractalgram Design Principles. Part I.

[Respect Game](concepts/respect-game.md) seems to be the core component of fractal organizations as we know them. The fractally whitepaper[^1] which inspired most of the ecosystem included many other features you would want in a DAO, and we have considered many more since, but Respect Game remains the one constant across time and different fractals that distinguishes them from other DAO initiatives. This highlights the importance of the app or tool we use for playing Respect Game.

We have experimented with multiple apps so far, but I think none of them "get" the Respect Game completely right yet. Also, different implementations of the process have appeared, some of which deviate from the original principles quite significantly.

In this article, I want to explore what makes Respect Game great, how current implementations sometimes miss the mark, and what design principles we should follow when building tools for it.

## How the Original Game Worked

The original mechanism for distributing Respect in fractals described in the fractally whitepaper [^1] (that we later called Respect Game [^7]) was elegantly simple in its design. The system did not prescribe *how* participants should build consensus—it only listened for the final consensus signal. If there was consensus, respect was distributed. If there was no consensus, no one received respect.

Crucially, the system did not provide a voting tool that determines who gets respect. It did not implement any tallying mechanism. Instead, people used discussion and negotiation to arrive at consensus.

The fractally whitepaper[^1] made this explicit:

> The group can use any process they like to reach consensus so long as everyone agrees by the end of the 1 hour window. ƒractally intentionally avoided implementing a voting and tallying system because all such systems encourage people to "vote strategically" instead of honestly. Instead the meeting should be a back and forth discussion and negotiation.
>
>The lack of a "voting" system means that people are forced to build trust that everyone is in agreement so that they can accurately report their opinion on the consensus. Building trust is a vital part of strong communities and identifying those who violate trust is critical to securing the integrity of a community.

The way we played Respect game in the first fractals reflected this. The [Respect](./concepts/respect.md) distribution was determined based on the signal from the participants of the breakout room. The system did not care how the signal was built [^2][^3].  That's how we were able to experiment with different ways to build consensus and come up with [fractalgram](./concepts/fractalgram.md) process in the first place.

## What Makes Respect Game Great

This is probably not an exhaustive list, but here are some of the things that make Respect Game great.

### Subjectivity

While a lot of the crypto world is trying to codify and automate things like impact measurement, public goods funding, and token distribution, fractals leave it to people—their collective consensus—to decide. Yes, this implies subjectivity, and while many would be quick to judge that as a weakness, it is actually a strength.

Not everything has an objective metric to measure it by. And even when there are metrics, you run into Goodhart's Law: *"When a measure becomes a target, it ceases to be a good measure." [^4]* Respect Game does not have this problem. A group of people playing Respect Game can consider a much wider range of signals, most of which are not and often cannot be on-chain. They can also adapt to different situations meeting by meeting. Public goods funding platforms that try to measure things objectively cannot compete with this level of awareness, flexibility, and adaptability [^5].

### Listening

Listening is something often lacking in organizations. Meetings become spaces where people wait for their turn to speak rather than genuinely engaging with others' ideas. When people do not feel heard, they disengage—and valuable perspectives are lost.

Respect Game not only enables listening but incentivizes it. Everyone gets the chance to speak—to present their contributions—and people have to listen. Why? Because they will need to reach consensus. If you have not paid attention to what others presented, you cannot meaningfully participate in the consensus-building process.

### Accuracy and Fairness Through Dialectic

Imagine one of the participants sees something important about a contribution that others miss. If there is no discussion, this information is not shared and the result will not take that data point into account. But if people discuss their rationale for their positions, you get the actual benefit of 3–5 people reviewing a single contribution—each bringing awareness to different pieces of information and different perspectives.

Furthermore, if this sharing of perspectives leads to actual discussion, where people move from disagreement to mutual agreement, there is a strong chance that the accuracy and fairness of the result will improve. This is the dialectic principle [^9]: by presenting and considering different viewpoints, we can arrive at a conclusion that is superior to any single perspective.

### Building Trust, Connection, and Understanding

Consensus building creates space for participants to express any sense of unfairness. People can advocate for themselves and for others. When disagreements are resolved through dialogue rather than algorithmic tallying, genuine trust develops between participants, instead of resentment. This trust becomes the foundation of a strong community.

In general, frequent interactions are necessary for building strong connections. Strong connections are the foundation of a strong community.


### Practicing Consensus Building

The incentive structure is clear: if we do not have consensus, no one gets respect. This creates a powerful motivation to actually work through disagreements rather than walk away from them.

Consensus building is a key skill in today's world. Most of our significant challenges—whether in organizations, communities, politics or society at large—require people to find common ground. Respect Game provides regular practice in this essential skill.

### Detecting Bad Behavior

Respect Game creates a space to detect bad behavior like cartels or selfish voting. When consensus is required and discussions are open, attempts to game the system become visible. Strategic manipulation is much harder when you must consistenly justify your positions publicly and reach genuine agreement with others.

## The Centrality of Consensus Building

Notice that at least four of the six benefits listed above depend directly on the need to build consensus. You do not get these benefits if you remove the requirement for consensus in Respect Game (e.g.: by creating a voting system that determines results from initial votes).

- **Listening**: If there is no need to build consensus, there is less incentive to listen. People can have a strategy prepared for voting for themselves and their friends regardless of what others present. There is no situation where they must share their opinion or present arguments for their position.

- **Accuracy and fairness**: This is the effect of consensus building. By synthesizing different perspectives, we can better approach the truth.

- **Building trust and connection**: Trust emerges from the process of working through disagreement together. Remove the need for consensus, and you remove the opportunity for trust to develop. In general, without consensus building, community will need less interaction and that will result in weaker connections which will result in weaker community.

- **Practicing consensus building**: Obviously, if you remove the need for consensus, you are no longer practicing it.

## How Implementations Deviate from These Principles

Unfortunately, many implementations of Respect Game have drifted away from what makes the game great.

### Voting Systems That Replace Consensus Building

Some apps implement Respect Game in a way where consensus building is replaced by a voting system that determines rankings from the initial votes of participants. Others assume that certain voting thresholds mean consensus has been reached.

Even when using an app that prioritizes consensus building, groups sometimes create voting systems themselves by, for example, applying a rule that 4 out of 6 votes means consensus.

If the goal is consensus, such thresholds can be a helpful heuristic, but we should not forget that they are *only* that. If someone has a good argument to vote otherwise, they should ideally get space to present that argument and try to change minds. **If the goal is consensus, then initial votes in Fractalgram just express everyone's initial opinions. They are the start of consensus building, not the end.**

We must leave space for the minority opinion to express itself in Respect Game. Not doing this creates room for bad emotions like resentment to build up over time.

### Anonymous Voting

When people can vote anonymously, they hide from the social responsibility of their votes. This leads to them not having to listen, not having to justify their positions, and not having to engage in genuine dialogue. The benefits of the dialectic process are lost.

### Lack of Discussion

When we rarely share perspectives and discuss—even with apps that technically support it—we miss most of the benefits of Respect Game. The tool might be capable, but if the practice does not include discussion, the value is not realized.

## Main Takeaways

Building consensus is hard. But we should thank that difficulty for many of the benefits of Respect Game. The challenge *is* the feature.

**The Respect Game process should be totally human-controlled. The role of the app is to help people communicate in building consensus themselves, not to make decisions for them.**

**The app should not tell the group when consensus is reached. People should determine that themselves.**


## Challenges

### How Should the App Determine When to Proceed?

If the process is human-controlled, how does the app know when to move to the next step of the [fractalgram](./concepts/fractalgram.md) process?

One approach is to require 100% consensus in the current step before proceeding. But this is not realistic in the real world. Network connection issues, people being away from keyboard, or being otherwise distracted mean that sometimes—from my experience, quite often—100% explicit agreement is impossible, and not because someone actively disagrees.

**A single moderator (host) who controls the process is the solution.**

### Do We Have to Trust the Host?

If one person controls the process, isn't this un-democratic? What if the host abuses their power?

The answer is to **make forking of the room easy**. If creation of new Respect Game rooms is simple, then whenever participants are unhappy with how the host controls the process, any single one of them can create a new room with better governance.

Note that this solution does not work as well if breakout room creation is coupled to the respect distribution mechanism—if your smart contracts create breakout rooms that can earn respect, then people cannot easily fork. Unless you create some other method for people to fork breakout rooms.

A simpler solution is what ORDAO fractals [^6] do: ORDAO (as the [executive branch](./concepts/tripartite-governance-model.md)) has its own consensus process that people use to execute results from Respect Game, regardless of how the rooms are created and managed. ORDAO listens to the final consensus and is agnostic to how this consensus was built. This is also aligned to [how original fractals worked](#how-the-original-game-worked).

### Does it mean less user friendly app?

The original fractalgram also needed admins who controlled the session flow. It also had a manual [^8] and people needed to be taught how to be admins (hosts mentioned above) of a group.

So if we stick with the host model, does that mean that we stick with that same lack of user-friendliness? No, we can make it user friendly by creating a special purpose UI for fractalgram process. A host would be able to control the process with only one or two buttons (forward and backward). At most, the host would need to know what Respect game is, ideally having played it at least once. The UI can make it obvious what to do at that point.

Having at least one existing respect-holder in every breakout room is desirable anyway.

## Conclusion

A Fractalgram session must be controlled by a human moderator. The app facilitates communication and helps issue final consensus signal—but humans determine what consensus is and when it has been reached.

This is a better solution even if we might prefer some kind of threshold of votes to determine when consensus is reached. The unreliable nature of an online game like this means we cannot consistently depend on voting thresholds. A human moderator can handle the messy realities of network issues, distracted participants, and genuine disagreement with the flexibility and judgment that no algorithm can match.

The power of Respect Game lies in the human process of consensus building. Our tools should support that process, not replace it.


[^1]: https://pink-squealing-leopon-501.mypinata.cloud/ipfs/QmdqiLhG3gEUFLGq2w5a1jzK4XmjJTFmabPvHUx2rGEkx9
[^2]: https://github.com/sim31/frapps/tree/fb7fc0b6ddd94082d47f2d73d16de4e2cd99bf7b/fractals/genesis-fractal
[^3]: https://github.com/sim31/frapps/tree/fb7fc0b6ddd94082d47f2d73d16de4e2cd99bf7b/fractals/eden-fractal
[^4]: https://en.wikipedia.org/wiki/Goodhart%27s_law
[^5]: Imagine a situation where someone is able to contribute well according to all objective metrics, but is toxic to the community in variety of other ways. This is just one example where Respect game would be able to take this into account things that are out of scope from any objective measurements.
[^6]: https://github.com/sim31/orfrapps
[^7]: https://optimystics.io/respectgame

[^8]: https://peakd.com/dao/@sim31/introducing-fractalgram

[^9]: https://www.qualityresearchinternational.com/socialresearch/dialectic.htm#:~:text=metaphysical%20realities.%20%5BEXPLAIN%5D-,Hegelian%20dialectic,-Dialectical%20analysis%20is