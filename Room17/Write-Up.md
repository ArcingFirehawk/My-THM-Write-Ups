# Threat Hunting: Introduction
+ Description: Behind the scenes of Threat Hunting - mindset, process, and goals.
+ Link: https://tryhackme.com/room/introductiontothreathunting
+ Type: Walkthrough
+ Completed: 2025-04-25

## Miscellaneous Abbreviations
+ IR = Incident Response
+ IOC = Indicators of Compromise

## Vocabulary
+ **Threat Hunting** An approach to finding cyber security threats where there’s an active effort done to look for signs of malicious activity.

## External Resources
+ MITRE ATT&CK Navigator ([Link](https://mitre-attack.github.io/attack-navigator/))

## Task 01 | Introduction
N/A

## Task 02 | Core Concepts
+ Incident Response (IR)
  + Reactive
  + Triggered by an initial alert. This alert is first triaged, then analysed, and when enough pieces of evidence point to malicious activity, it’s deemed an incident that needs to be responded to and dealt with accordingly.
  + “There's a threat that needs to be dealt with now.”
+ Threat Hunting
  + Proactive
  + There’s no no actual “trigger” that would mobilise a hunt, except for the pursuit of building the strength of the organisation’s security posture.
  + Guided by Threat Intelligence.
  + “There might be a threat that we don't know yet.”
+ Usually, organisations start doing threat hunts when there’s already an established IR process and as detection mechanisms in place, but they think that incidents aren’t being detected early enough. In the case of advanced threats there will always exist ways to go through your organisation undetected.
  + Threat Hunting aims to bridge this gap by constantly finding ways to add and improve the current detection mechanisms in place so that future similar bad behaviour will automatically be detected immediately.
  + During that process, detected threats go immediately to the IR team. The trigger for their mobilisation are the findings from the Threat Hunt, and consequent findings from the IR process may steer the Threat Hunting team to further find bad behaviour.

## Task 03 | Threat Hunting Mindset
+ People would always be the most important part of any security team.
+ It’s imperative to start hunts with leads comprised of accurate information (e.g., known relevant malware and trusted Threat Intelligence).
+ It’s essential that we arm ourselves with critical information that will let us know more about the threat(s) that we may be dealing with. Understanding what we may be dealing with is akin to knowing how they might behave within our environment.
+ Intelligence on threats that you are able to develop internally is a very valuable asset.
  + Intelligence of this kind may have the characteristic of being ultimately unique to your organisation.
+ IOCs immediately give value not only to your threat hunters but also to your detection mechanism, as they’re actual traces of an adversary. Through this, re-intrusions of that specific adversary or other adversaries that employ the same tactics would be easier to spot.
+ Not a lot of organisations are capable of developing valuable and actionable Threat Intelligence internally. It involves a lot of money, skill, and effort to be able to become an efficient Threat Intelligence producer.
+ Organizations can learn from Threat Intelligence producers via Threat Intelligence feeds.

## Task 04 | Threat Hunting Process
+ “What do we hunt for?”
  + Dictates the direction of the hunt.
  + As the hunt progresses, the threat hunter will always go back to this question, ensuring that pieces of evidence that link to its answer are gathered.
+ Know relevant malware, attack residues, and the vulnerabilities of your products/applications.
  + Leverage malware samples and their publicly available analyzes to identify the relevant threat actors that might take an interest in you, and hunt for traces of the malware that they use in their toolkits within your organisation.
  + Attack residue is a great starting point, but knowing your environment well enough to be able to separate attack residues from normal behaviour is a challenge.
+ “How do we hunt for it?”
  + Reviewing the array of information, factors, and other elements would hopefully lead to understanding the target of the hunt.
+ Upon identifying the subject of the hunt, it’s imperative to characterize them into specific and actionable identifiers that can be immediately recognized.
  + Done most effectively via Attack Signatures and IOCs.
  + By condensing the “whats” of the hunt down to Attack Signatures and IOCs, we suddenly have a set of information that we can then immediately compare to our available historical data.
  + Makes it easier to find objects of interest.
+ Some hunting projects are best accomplished via logical queries (e.g., hunting for assets that have known vulnerabilities).
+ After narrowing down narrowed down the specific bad to focus on, the next sensible step is to characterise their behaviour through patterns of activity that they’re inclined to make.

## Task 05 | Practical Application
+ The MITRE ATT&CK Navigator is a tool designed to make it easier “to visualise your defensive coverage, your red/blue team planning, the frequency of detected techniques or anything else you want to do.”
  + It shows the specific attack techniques that we should look for and gives an idea of how an attack flows in a visually appealing way.
+ The differing colours are set to be able to immediately see which common tactics or techniques threat actors share.
  + A deep red means that it’s common to all of them, while the other lighter colours mean it’s either one or any combination of two threat actors.
  + The scores are arbitrary, and you can set them in terms of relevance to your organisation or just random if you just intend to play around with the tool visually.
+ These tools and resources at hand would allow for a more straightforward approach to identifying patterns of activity.
+ This intelligence-driven approach of characterising threat actor behaviour through their TTPs is one of, if not the best, way to go about hunting. It immediately gives value to the hunt, and it’s sensible, straightforward, and actionable.

## Task 06 | Goals
+ Proactive Approach to Finding Bad
+ Discover Pre-existing Bad
+ Minimise the Dwell Time of Attackers
+ Develop Additional Detection Methods

## Task 07 | Conclusion
N/A