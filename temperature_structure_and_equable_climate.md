# Variation of Temperature with Season and Latitude and the Equable Climate Problem

These are my notes on what determines the geographic structure of surface temperature on a planet. The things I write here are for my own use and may be incorrect. Proceed with caution, these are written by someone who has never studied climate science before.

Last Edited 30/09/19 by Brett Mckim (brettmckim@gmail.com). I welcome questions and comments.

## Overview


### Distribution of Incident Solar Radiation and Temperature Structure

The *insolation* is the distribution of incident sun light. What determines the amount of incident radiation on the surface is that area's *zenith angle* denoted by zeta. 

![enter image description here](https://lh3.googleusercontent.com/PZ00fOvWx_DsfyQD_FDL5UO574TtDj9NVxkJkK7Ri5AcoUw2iriBfgF6S8emOPX4sA41TZlxgMLctQ)

In the relatively simple configuration where a planet is orbiting in a plane perpendicular to its rotational axis, the zenith angle is simply the planet's latitude $\phi$. We know from energy balance that the planet's surface temperature $T$ is proportional to the insolation $L_\odot^{1/4}$, so it should come as no surprise we get the following scaling when the variation of temperature with latitude is taken into account:

$$T(\phi) \sim (L_\odot \cos \phi)^{1/4}$$ 

This gives us 2 pieces of insight:

1) That the first order contribution to a planet's temperature structure is determined by the insolation.

2) That this contribution is incomplete in describing planets such as Earth. In the limit as $\phi \rightarrow \pi/2$, the clearly unphysical result is that $T \rightarrow 0$. There must be other mechanisms of transferring heat, namely radiative cooling, convective motions, and the presence of an atmosphere (even a static one).

Although radiation is the first component in determining a planet's equilibrium temperature profile, it is not the whole story and an account of the convective transfer of heat is necessary. 

## The Equable Climate Problem

An *Equable Climate* is a climate where the equator to pole temperature difference (EPTD) is low and there is very little seasonality. This means the temperature is roughly the same around the globe and winters are similar to summers.

So what do our observations tell us about the Eocene?

> Sea surface temperatures at the North Pole increased to 23°-24°C in the Eocene epoch (~56 Ma to 34 Ma) (Sluijs et al., 2006; Moran et al., 2006). In North America between 45°N-50°N, there were no periods longer than a day with temperatures below freezing, and no minimum temperatures below -10°C, or 14°F (Wing and Greenwood, 1993). Additionally, during this time, Arctic sea surface temperatures reached tropical or subtropical levels (Sluijs et al., 2006). These details reveal the extent of the differences between the modern climate and the equable climates in the Cretaceous and Eocene.

> Additionally, ancient crocodilians, relatives to present-day crocodiles and alligators, and giant tortoises lived in Wyoming (latitude 47°N) and even Ellesemere Island (latitude 78°N), places far too cold for them now (Wing and Greenwood, 1993; Markwick 1994).

Wow that is pretty different from today! **Would be great to have a graphic of Global Surface Temperatures during Eocene vs Now.** The problem is that:

> Modelling studies of the early Eocene over the past decades have consistently failed to reproduce the warm continental interior temperatures inferred from paleoclimate proxies, an issue that has come to be known as the “equable climate problem” (Sloan and Barron, 1990, 1992; Sloan, 1994). The model-data mismatch is typically ∼20◦C for winter temperatures; for mean annual temperature (MAT) the error is typically less, but reaches 10–20 C near the poles (Shellito et al., 2003; Huber et al., 2003; Winguth et al., 2010; Roberts et al., 2009; Shellito et al., 2009).
> - Huber et al., 2010

Here is the problem framed by Raymond Pierrehumbert: 

> The outstanding problem of hothouse climates, however, is to account for the apparently low pole to equator temperature gradient characteristic of hothouse climates. A related and even more perplexing problem is how to account for the absence of cold continental interior winters. Both issues are unresolved, and both engage fluid dynamical phenomena [...] It is [...] maybe even likely, that clouds might play a role in smoothing out the geographical variations in radiative forcing that would normally lead to cold winters and cold poles or hot tropics.

### Previous Work and Theories

Clearly people have given a lot of time and thought to this. What are proposed reasons?

> large lakes; polar stratospheric clouds; increased ocean heat transport; finer resolution simulations of continental interiors; a permanent, positive phase of the Arctic Oscillation; altered orbital parameters; altered topography and ocean gateways; radiative convective feedbacks; altered vegetation; and changes in sea surface temperature (SST) distributions.

However, it seems that most of these are missing some essential bit as they still fail to reproduce the Eocene climate. Even increasing CO2 tends to warm the tropics too much and the poles not enough. Although Huber et al., 2010 seems to think otherwise:

> We have shown that increased radiative forcing in the form of very high pCO2 seems to resolve the equable climate problem without running too far afoul of other constraints

Although they admit that:

> We have not addressed whether the enhanced radiative forcing was due to pCO2, methane, other greenhouse gases, novel cloud feedbacks, or other “missing” factors. We have also not established whether large forcing is actually necessary, the alternative being high values of climate sensitivity as in the study of Heinemann et al. (2009) and only moderate increases in forcing

And although they might be able to reproduce *some* aspects of the Eocene climate, they don't really understand *why* they get some things right and some things wrong. So that means there is still plenty of work to be done, because there is a difference between simulating and understanding.

### My Role (I Think)

Using an idealized climate model, can the ocean and atmosphere make significant heat transport to change the equator-pole temperature gradient and seasonality. Specifically I want to understand the level of meridional heat transport that is required to obtain a high latitude warm ocean temperatures, low temperature gradients, and warm continental winters.

I'd probably start with an atmosphere only model with imposed ocean heat transport. I'd begin with 3 control integrations with present level CO2 with no ocean heat fluxes, observed fluxes, and fluxes that should give us today's values. With this model we don't worry about topography and no ocean feedbacks are included. I shouldn't forget to vary other planetary parameters like rotation rate and atmospheric thickness!

Here is how the NSF-NERC research proposal puts it:

> Objective 3: Understand the level of meridional heat transport by the ocean and/or atmosphere that is required to obtain a high latitude warm ocean temperatures and low equator to pole temperature difference and sufficiently warm continental winters.

> Hypotheses: (H3) A sufficiently large level of ocean heat transport in conjunction with high CO2 levels and polar amplification can give rise to a small low-level meridional temperature gradient but of itself is unlikely to give rise to sufficiently warm continental interiors in winter in extreme warm periods.

> Significance: Understanding the contributions and limitations of particular mechanism is important to untangling the multiple factors responsible to producing a given climate.

> Practical steps: (i) Use a flexible GCM (e.g., Isca) with imposed ocean heat transport and a variety of continental configurations and land surfaces. Use a coupled version of the model (with a dynamical ocean) to see how viable the ocean heat transports are. Analyze the contributions and feedbacks associated with individual mechanisms.

Please note that we will differ in our goal and approach from most of the studies I have read:
> Although we are informed by these climates, and we will compare model outputs to paleo-data where relevant, our primary goal is not to reproduce a specific past climate with an Earth System model. Rather, we have the more general goal of understanding the dynamics and other mechanisms that can give rise to such climate regimes, and in particular what surprises they may induce.

## Other Notes

> Looking at my Frierson notes, we can see a clear difference in temperature between the equator and the poles. **But can we derive this difference from from first principles?**

