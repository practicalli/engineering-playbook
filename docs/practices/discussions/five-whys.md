# Five Whys

Five whys is a simplistic technique to explore cause-and-effect relationships underlying a particular problem.

The primary goal is to quickly determine the root cause of an issue by repeating the question "Why?" five times. The answer to the fifth why should reveal the root cause of the problem.

!!! WARNING "5 Whys may not be enough"
    More questions or a deeper analysis may be required to find the actual root cause, especially for more intricate challenges

    Process or systemic issues can often generate responses about lack of time, resources or understanding.  Whilst these may be an effect, they are not specific enough to be the root cause.

The troubleshooter should avoid assumptions and logic traps and focus on tracing the chain of causality in increments to a root cause which has connection to the original problem.

> The technique was described by Taiichi Ohno at Toyota Motor Corporation.

## Example

Challenge: The web service is not responding

1. Why? - the service monitor displays an error
2. Why? - pinging the heartbeat of the service returns a 404
3. Why? - the request is not recognised by the server
4. Why? - the heartbeat url has changed
5. Why? - a change was made to the code but monitor configuration not updated

This example shows a disconnect between development and operations

!!! WARNING "Five Whys can feel negative to the reciever"
    Practice the technique on yourself to understand how the tone of questions can be percieved as annoying (negative) or curious (constructive)

[:globe_with_meridians: Five Whys - Wikipedia](https://en.wikipedia.org/wiki/Five_whys
){target=_blank .md-button}

[:globe_with_meridians: Root Cause Analysis - Wikipedia](https://en.wikipedia.org/wiki/Root_cause_analysis){target=_blank .md-button}
