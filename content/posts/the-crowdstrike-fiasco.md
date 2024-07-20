+++
title = 'The Discussion Around The CrowdStrike Fiasco And What We Should Learn From It'
date = 2024-07-20T19:19:19+02:00
published = true
tags = ['Open Source Software', 'Software Engineering', 'CrowdStrike', 'Security', 'Opinion']
+++

This blog post stems from a conversation with a friend about the recent CrowdStrike debacle. Besides many excellent sources online, I highly recommend [this Low Level Learning video](https://youtu.be/pCxvyIx922A?si=hOfO_x8bf0RiaxCG) to get a concise technical overview of the incident.

## Introduction

Within the Dev community, I've spotted three primary criticisms regarding the CrowdStrike incident:
1. Why wasn't there sufficient testing?
2. Why was the rollout done to 100% of devices and not incrementally?
3. Why is everyone using Windows?

## Testing Environments and Quality Control

Many argue that thorough testing could have prevented this scenario. I largely agree. The final testing environment should mirror the production environment as closely as possible. This means using real machines, real networks, and real Windows installations. Earlier tests can run on virtual machines, but the last step should be as realistic and ideally automated as possible. A billion-dollar company should adhere to the highest standards here.

In CrowdStrike's case, a corrupted file (all zeros) was shipped, which crippled all systems that had the driver installed, attempting to dereference non-existent memory. Wow.

The argument that you can't test everything is valid to an extent. However, the fact that an update sent out a corrupted file should have been caught. Especially with a company of this scale and importance. I expect them to go above and beyond in Quality Assurance, given their very high responsibility.

## Rollout Strategy

Another major criticism concerns the rollout strategy. Why push the update to 100% of devices all at once instead of doing it incrementally? An incremental rollout would allow the company to catch issues on a smaller scale before they affect the entire user base.

This phased approach is a standard practice in software deployment, aimed at minimizing risk. It's surprising that CrowdStrike did not employ this strategy, especially considering the potential impact of a faulty update. By rolling out to a small percentage of devices first, any critical issues could be identified and addressed promptly, preventing widespread disruption.

Multiple levels of checks and balances should prevent such a fatal error from occurring in production devices. It’s evident that CrowdStrike failed to implement sufficient processes to avoid such an incident. Mistakes can happen, and they do all the time. However, I (and many others) expect more from a company of this size and stock market value. There are technical, engineering, and organizational measures that can be put in place to prevent such events, and it’s disappointing that these weren’t effectively utilized here.

## Why Windows?

Perhaps the most intriguing point: Why Windows in the first place? It remains partly a mystery to me, although I can see some explanations. Yes, Windows is widespread in consumer and enterprise environments. But why it runs on displays or servers baffles me. Maybe someone in the company was well-versed in Windows, and the management decided, "Linux? Never heard of it. Windows, I know that, so let's use that!" The Windows lobby is certainly stronger, as it’s more profitable.

For systems that just need to boot up and run a single program, there's no compelling reason to use Windows. A minimal Linux setup would be ideal here. Windows often brings unnecessary features and may require additional antivirus software to ensure security.

I'm not just talking about digital signage (screens with ads or flight information), but also about systems like airport boarding computers or point-of-sale systems. These typically just need to boot up quickly and run a single application. No need for Word, Teams, or Excel. A minimal Linux setup can do this perfectly.

The main issue is that Linux, being open-source, doesn't have the same lobby or service providers, even though, in my opinion, it's the superior OS, especially for the use cases mentioned above.

## Conclusion and Outlook

Lastly, we should consider ourselves lucky that this was "just" a bug and not an exploit. A corresponding security vulnerability spreading to millions of machines in critical enterprise sectors, taking over the PC at the kernel level, would be catastrophic and far more damaging. The question remains: How much responsibility for global business and organizational processes should we place in the hands of a single private company?

This case highlights the potential scale of a security disaster with a provider like CrowdStrike. It rightly sparked discussions about the openness of such software. Some are calling for CrowdStrike to be mandated to open their datasets and possibly even AI models used for predicting and assessing network activities. I hope this discussion doesn't fade away as the incident is forgotten in a couple of weeks.

In closing, I urge the community and readers to actively participate in this discussion. We must ask ourselves: At what cost does a company like CrowdStrike gain this power? I believe it would be much safer and more sustainable to develop and promote open security software based on an open operating system, supported by capable service providers who might eventually create their own lobby.