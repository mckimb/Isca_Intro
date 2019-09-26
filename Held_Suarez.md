# What *Is* the Held-Suarez Configuration?

These are my notes on what the Held-Suarez Configuration *is* and what it is used for in climate modeling. The things I write here are for my own use and may be incorrect. Proceed with caution, these are written by someone who has never studied climate science before.

Last Edited 25/09/19 by Brett Mckim (brettmckim@gmail.com). I welcome questions and comments.

## Purpose of the Model

The Held-Suarez Configuration (HS) is an idealized climate model which focuses primarily on the dynamics of a climate (ie. the circulation) as opposed to other aspects like radiation and clouds. It was designed to make it easier to compare different climate models and see which one reproduces the climate statistics more readily.

HS is designed to evolve the climate toward a statistically steady state, by enforcing a specific temperature profile (as a function of height and latitude) and damped dynamics near the surface. This imposing of temperature and damping will create an equilibrium state. I'll quote directly from Isaac Held in his blog article, [The "fruit fly" of climate models](https://www.gfdl.noaa.gov/blog_held/28-the-fruit-fly-of-climate-models/).

> The model is designed to capture some of the complexity of midlatude jets and storms tracks on a rotating sphere. The climate that emerges (the statistics of the winds and temperatures) has a lot of features that are quite Earth-like.

If the dynamics were turned off, the climate would evolve to the state imposed by the "boundary conditions" so to speak.

## What can we vary in the Model?

A lot! Here's a whole list so lets start from top to bottom.

**Newtonian Cooling** states that the rate of change of the temperature of an object is proportional to the difference between its own temperature and the ambient temperature. We can set the timescale of this proportionality.

**Damping Order** parameterizes diffusive and convective processes associated with clouds. (I think)

**Scale Heights** controls the stratification.

**Sigma levels** is just the pressure at a particular height relative to the surface  pressure.

**Boundary Layer Friction Height** says what it is.

**Resolution** is usually denoted by a "T" because in spherical harmonics we are restricted in the number of modes that are available (remember the hydrogen atom?). So T42 for example means 128 longitudinal by 64 latitudinal points. I would google it for more examples. 

And so on. Not too bad! The hard part is to determine which set of parameters gives you a reasonable climate. These are knobs to be adjusted in an ad-hoc matter, by intuition, observations, and even from first principles. Perhaps one becomes more (or perhaps less!) sophisticated in their methods of setting parameters as they go on.



```python
'atmosphere_nml': {
'idealized_moist_model': False  # False for Newtonian Cooling.  True for Isca/Frierson
},

'spectral_dynamics_nml': {
'damping_order' : 4, # default: 2
'water_correction_limit' : 200.e2, # default: 0
'reference_sea_level_press': 1.0e5, # default: 101325
'valid_range_t' : [100., 800.], # default: (100, 500)
'initial_sphum' : 0.0, # default: 0
'vert_coord_option' : 'uneven_sigma', # default: 'even_sigma'
'scale_heights': 6.0,
'exponent': 7.5,
'surf_res': 0.5
},

# configure the relaxation profile
'hs_forcing_nml': {
't_zero': 315., # temperature at reference pressure at equator (default 315K)
't_strat': 200., # stratosphere temperature (default 200K)
'delh': 60., # equator-pole temp gradient (default 60K)
'delv': 10., # lapse rate (default 10K)
'eps': 0., # stratospheric latitudinal variation (default 0K)
'sigma_b': 0.7, # boundary layer friction height (default p/ps = sigma = 0.7)

# negative sign is a flag indicating that the units are days
'ka': -40., # Constant Newtonian cooling timescale (default 40 days)
'ks':  -4., # Boundary layer dependent cooling timescale (default 4 days)
'kf': -1., # BL momentum frictional timescale (default 1 days)
'do_conserve_energy': True, # convert dissipated momentum into heat (default True)
},
```

## What does the Model Create?

Most of the parameters of the model are chosen so as to prevent any sort of instability like an inversion layer. Therefore we get a stable climate emerging where we can measure various statistics. **Note that there is no seasonal variation in the default HS model and that the first 2 years of the 10 year run are thrown out due to the spinup period.**  Side note: Maybe this is how we determine the spinup period, run a model in HS configuration and see how long it takes to equillibriate to the prescribed temperature profile. 

Lets do some basic visualizations to aid these sections.

### Seasonally and Zonally Averaged Zonal Wind

This gives us a contour plot of zonal (east-west) winds and shows us some interesting things about the climate: that winds often get concentrated into narrow regions known as jets. The HS model produces 2 midlatitude jets, both in the Eastward direction. Why? Check out Chapter 12 of Geoff's new book. I will add to this section once I understand it better. **Why are both the midlatitude jets eastward?**

![enter image description here](https://lh3.googleusercontent.com/TNdiYZyj34kdd1ReBILdJkgjnzuspiS1Xmeu1NT0UkgNDysfIxttKolGs3Svf9bXROtTSwAudzqf1Q)

### Veritical Temperature Profile of the Atmosphere

The atmosphere is made of an ideal dry gas so we should expect a standard adiabatic lapse rate. **It would be cool to confirm the lapse rate measured in ISCA agrees with the lapse rate derived from theory** 

One question I have is: **Is pressure a monotonic function always?** Because I could imagine on a planet with strong winds there no longer is a 1 to 1 correspondence with height.

![enter image description here](https://lh3.googleusercontent.com/WqHw1cdL4Y-zssoHEMM41x0bWg6SQ7-3yC8Djn3iX1An7koCYOzELF4jiC7uTuNi0BAx1hoOGrkmkQ)


### Meridional Temperature Profile of the Atmosphere

The plots show the equator-pole temperature gradient quite nicely, but the most interesting thing to me is that the equator is actually cooler than the sub-tropics. **Why is the subtropics warmer than the equator? Is this feature robust across different models?**

![enter image description here](https://lh3.googleusercontent.com/AVaGIQ_2vjwffKOOYzhTnffnRDW6peoszTpIAUGEOap_G2SbpZuftphyyCCcQK7HrKCCPRXXUevMVw)

## Moving up the Model Hierarchy: The Next Step

The Frierson Model?
