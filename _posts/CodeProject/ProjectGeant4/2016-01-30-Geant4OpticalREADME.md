---
layout: post
title: "Geant4 Optical User's Guide"
date: 2016-01-30 12:47:57 +0800
comments: true
tags: [Geant4]
---
<!-- README.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 六 1月 30 12:47:57 2016 (+0800)
;; Last-Updated: 六 7月 16 20:02:48 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 31
;; URL: http://wuhongyi.cn -->

# 材料的光学属性

通过 G4MaterialPropertiesTable 与 G4Material 连接。

其中，必须要设置的参数：SCINTILLATIONYIELD、RINDEX、ABSLENGTH。


没有 FASTCOMPONENT、SLOWCOMPONENT 属性不产生光子。

# 提供的光学过程

- G4OpAbsorption
- G4OpRayleigh
- G4OpMieHG
- G4OpBoundaryProcess
- G4OpWLS
- G4Scintillation
- G4EmSaturation
- G4Cerenkov

<!--more-->
## Cerenkov

The radiation of Cerenkov light occurs when a charged particle moves through a dispersive medium faster than
the group velocity of light in that medium. Photons are emitted on the surface of a cone, whose opening angle
with respect to the particle's instantaneous direction decreases as the particle slows down. At the same time, the
frequency of the photons emitted increases, and the number produced decreases. When the particle velocity drops
below the local speed of light, the radiation ceases and the emission cone angle collapses to zero. The photons
produced by this process have an inherent polarization perpendicular to the cone's surface at production.

The time and position of Cerenkov photon emission are calculated from quantities known at the beginning of a
charged particle's step. The step is assumed to be rectilinear even in the presence of a magnetic field. The user may
limit the step size by specifying a maximum (average) number of Cerenkov photons created during the step, using
the **SetMaxNumPhotonsPerStep(const G4int NumPhotons)** method. The actual number generated
will necessarily be different due to the Poissonian nature of the production. In the present implementation, the
production density of photons is distributed evenly along the particle's track segment, even if the particle has
slowed significantly during the step. The step can also be limited with the **SetMaxBetaChangePerStep**
method, where the argument is the allowed change in percent).

## Scintillation

Every scintillating material has a characteristic light yield, **SCINTILLATIONYIELD**, and an intrinsic resolution,
**RESOLUTIONSCALE**, which generally broadens the statistical distribution of generated photons. A wider
intrinsic resolution is due to impurities which are typical for doped crystals like NaI(Tl) and CsI(Tl). On the
other hand, the intrinsic resolution can also be narrower when the Fano factor plays a role. The actual number
of emitted photons during a step fluctuates around the mean number of photons with a width given by
**ResolutionScale\*sqrt(MeanNumberOfPhotons)**. The average light yield, **MeanNumberOfPhotons**,
has a linear dependence on the local energy deposition, but it may be different for minimum ionizing and
non-minimum ionizing particles.

A scintillator is also characterized by its photon emission spectrum and by the exponential decay of its time spectrum.
In GEANT4 the scintillator can have a fast and a slow component. The relative strength of the fast component
as a fraction of total scintillation yield is given by the **YIELDRATIO**. Scintillation may be simulated by specifying
these empirical parameters for each material. It is sufficient to specify in the user's **DetectorConstruction**
class a relative spectral distribution as a function of photon energy for the scintillating material. 

In cases where the scintillation yield of a scintillator depends on the particle type, different scintillation processes
may be defined for them. How this yield scales to the one specified for the material is expressed with the **ScintillationYieldFactor**
in the user's **PhysicsList**. In those cases where the
fast to slow excitation ratio changes with particle type, the method **SetScintillationExcitationRatio**
can be called for each scintillation process (see the advanced underground_physics example). This overwrites the
**YieldRatio** obtained from the **G4MaterialPropertiesTable**.

A Gaussian-distributed number of photons is generated according to the energy lost during the
step. A resolution scale of 1.0 produces a statistical fluctuation around the average yield set with
**AddConstProperty("SCINTILLATIONYIELD")**, while values > 1 broaden the fluctuation. A value of
zero produces no fluctuation. Each photon's frequency is sampled from the empirical spectrum. The photons
originate evenly along the track segment and are emitted uniformly into 4π with a random linear polarization and at
times characteristic for the scintillation component.

When there are multiple scintillators in the simulation and/or when the scintillation yield is a non-linear function of
the energy deposited, the user can also define an array of total scintillation light yields as a function of the energy
deposited and particle type. The available particles are protons, electrons, deuterons, tritons, alphas, and carbon
ions. These are the particles known to significantly effect the scintillation light yield, of for example, BC501A
(NE213/EJ301) liquid organic scintillator and BC420 plastic scintillator as function of energy deposited.

The method works as follows:

- In the user's physics lists, the user must set a G4bool flag that allows scintillation light emission to depend
on the energy deposited by particle type: **theScintProcess->SetScintillationByParticleType(true);**

- The user must also specify and add, via the AddProperty method of the MPT, the scintillation light yield as
function of incident particle energy with new keywords, for example: PROTONSCINTILLATIONYIELD
etc. and pairs of protonEnergy and scintLightYield.

### 参数设置

产生光子必须有：

- FASTCOMPONENT、SLOWCOMPONENT
- RESOLUTIONSCALE

其中FASTCOMPONENT、SLOWCOMPONENT至少有一个。

----


FASTCOMPONENT、SLOWCOMPONENT分情况（里面还涉及考虑不考虑FiniteRiseTime）：

- 只有FASTCOMPONENT时
 - FASTTIMECONSTANT ：发光延迟为 -FASTTIMECONSTANT*std::log(G4UniformRand())
 - 如果FiniteRiseTime时，FASTSCINTILLATIONRISETIME ：发光延迟为sample_time(FASTSCINTILLATIONRISETIME, FASTTIMECONSTANT)
- 只有SLOWCOMPONENT时
 - SLOWTIMECONSTANT ：发光延迟为 -SLOWTIMECONSTANT*std::log(G4UniformRand())
 - 如果FiniteRiseTime时，SLOWSCINTILLATIONRISETIME ：发光延迟为sample_time(SLOWSCINTILLATIONRISETIME, SLOWTIMECONSTANT)
- 两者都有时。将计算光子数按比例分开，然后按照按照以上单成分计算
  - YIELDRATIO（快成分所占比例）
  - ExcitationRatio ：如果 ( fExcitationRatio == 1.0 || fExcitationRatio == 0.0)，快成分所占比例为 std::min(yieldRatio,1.0)；如果 （0<fExcitationRatio<1），快成分所占比例为 std::min(fExcitationRatio,1.0)。



如果只有快慢成分中的一种（占主要，另一种可以忽略），如果忽略上升时间，则发光延迟为该成分衰减时间分布的抽样，如果考虑上升时间，那么发光延迟为上升、衰减时间分布的抽样。

位置在步长内均匀抽样，时间为粒子到该位置的时间加上发光时间延迟。方向各向同性，确定偏振（polarization），能量抽样按照闪光光谱抽样。


----

发光与粒子种类有关时：

- PROTONSCINTILLATIONYIELD
- DEUTERONSCINTILLATIONYIELD
- TRITONSCINTILLATIONYIELD
- ALPHASCINTILLATIONYIELD
- IONSCINTILLATIONYIELD
- ELECTRONSCINTILLATIONYIELD

----

发光与粒子种类无关时（分考虑不考虑EmSaturation）：

- SCINTILLATIONYIELD
- YieldFactor

----

能损计算的光子数考虑展宽：

MeanNumberOfPhotons > 10 时，sigma = ResolutionScale * std::sqrt(MeanNumberOfPhotons); NumPhotons = G4int(G4RandGauss::shoot(MeanNumberOfPhotons,sigma)+0.5);   
MeanNumberOfPhotons <= 10 时，NumPhotons = G4int(G4Poisson(MeanNumberOfPhotons));   
NumPhotons <= 0， return unchanged particle and no secondaries。

----

## Wavelength Shifting

Wavelength Shifting (WLS) fibers are used in many high-energy particle physics experiments. They absorb light
at one wavelength and re-emit light at a different wavelength and are used for several reasons. For one, they tend
to decrease the self-absorption of the detector so that as much light reaches the PMTs as possible. WLS fibers are
also used to match the emission spectrum of the detector with the input spectrum of the PMT.

A WLS material is characterized by its photon absorption and photon emission spectrum and by a possible time
delay between the absorption and re-emission of the photon. Wavelength Shifting may be simulated by specifying
these empirical parameters for each WLS material in the simulation. It is sufficient to specify in the user's
**DetectorConstruction** class a relative spectral distribution as a function of photon energy for the WLS material.
WLSABSLENGTH is the absorption length of the material as a function of the photon's energy. WLSCOMPONENT
is the relative emission spectrum of the material as a function of the photon's energy, and
WLSTIMECONSTANT accounts for any time delay which may occur between absorption and re-emission of the photon.

The process is defined in the PhysicsList in the usual way. The process class name is G4OpWLS. It should be
instantiated with theWLSProcess = new G4OpWLS("OpWLS") and attached to the process manager of the optical
photon as a DiscreteProcess. The way the WLSTIMECONSTANT is used depends on the time profile method
chosen by the user. If in the PhysicsList the WLSProcess->UseTimeGenerator("exponential") option is set, the
time delay between absorption and re-emission of the photon is sampled from an exponential distribution, with the
decay term equal to WLSTIMECONSTANT. If, on the other hand, theWLSProcess->UseTimeGenerator("delta")
is chosen, the time delay is a delta function and equal to WLSTIMECONSTANT. The default is "delta" in case
the G4OpWLS::UseTimeGenerator(const G4String name) method is not used.


## Absorption

The implementation of optical photon bulk absorption, **G4OpAbsorption**, is trivial in that the process merely
kills the particle. The procedure requires the user to fill the relevant **G4MaterialPropertiesTable** with
empirical data for the **absorption** length, using ABSLENGTH as the property key in the public method **AddProperty**.
The absorption length is the average distance traveled by a photon before being absorpted by the medium;
i.e. it is the mean free path returned by the **GetMeanFreePath** method.

## Rayleigh Scattering

The differential cross section in Rayleigh scattering, is proportional to 1+cos2(θ), where θ is the polar of
the new polarization vector with respect to the old polarization vector. The **G4OpRayleigh** scattering process
samples this angle accordingly and then calculates the scattered photon's new direction by requiring that it be
perpendicular to the photon's new polarization in such a way that the final direction, initial and final polarizations
are all in one plane. This process thus depends on the particle's polarization (spin). The photon's polarization is
a data member of the **G4DynamicParticle** class.

A photon which is not assigned a polarization at production, either via the **SetPolarization** method
of the **G4PrimaryParticle** class, or indirectly with the **SetParticlePolarization** method of the
**G4ParticleGun** class, may not be Rayleigh scattered. Optical photons produced by the **G4Cerenkov** process
have inherently a polarization perpendicular to the cone's surface at production. Scintillation photons have a
random linear polarization perpendicular to their direction.

The process requires a **G4MaterialPropertiesTable** to be filled by the user with Rayleigh scattering length
data. The Rayleigh scattering attenuation length is the average distance traveled by a photon before it is Rayleigh
scattered in the medium and it is the distance returned by the **GetMeanFreePath** method. The **G4OpRayleigh**
class provides a **RayleighAttenuationLengthGenerator** method which calculates the attenuation coefficient
of a medium following the Einstein-Smoluchowski formula whose derivation requires the use of statistical
mechanics, includes temperature, and depends on the isothermal compressibility of the medium. This generator is
convenient when the Rayleigh attenuation length is not known from measurement but may be calculated from first
principles using the above material constants. For a medium named Water and no Rayleigh scattering attenutation
length specified by the user, the program automatically calls the **RayleighAttenuationLengthGenerator**
which calculates it for 10 degrees Celsius liquid water.


## Mie Scattering

Mie Scattering (or Mie solution) is an analytical solution of Maxwell's equations for scattering of optical photons
by spherical particles. It is significant only when the radius of the scattering object is of order of the wave length.
The analytical expressions for Mie Scattering are very complicated since they are a series sum of Bessel functions.
One common approximation made is call Henyey-Greenstein (HG). The implementation in Geant4 follows the HG
approximation (for details see the Physics Reference Manual) and the treatment of polarization and momentum
are similar to that of Rayleigh scattering. We require the final polarization direction to be perpendicular to the
momentum direction. We also require the final momentum, initial polarization and final polarization to be in the
same plane.

The process requires a G4MaterialPropertiesTable to be filled by the user with Mie scattering length data (entered
with the name: MIEHG) analogous to Rayleigh scattering. The Mie scattering attenuation length is the average
distance traveled by a photon before it is Mie scattered in the medium and it is the distance returned by the
GetMeanFreePath method. In practice, the user not only needs to provide the attenuation length of Mie scattering, but
also needs to provide the constant parameters of the approximation: g\_f, g\_b, and r\_f. (with AddConstProperty
and with the names: MIEHG\_FORWARD, MIEHG\_BACKWARD, and MIEHG\_FORWARD\_RATIO, respec-
tively; see Novice Example N06.)


# Boundary Process

For the simple case of a perfectly smooth interface between two dielectric materials, all the user needs to provide
are the refractive indices of the two materials stored in their respective **G4MaterialPropertiesTable**. In
all other cases, the optical boundary process design relies on the concept of *surfaces*. The information is split into
two classes. One class in the material category keeps information about the physical properties of the surface itself,
and a second class in the geometry category holds pointers to the relevant physical and logical volumes involved
and has an association to the physical class. Surface objects of the second type are stored in a related table and
can be retrieved by either specifying the two ordered pairs of physical volumes touching at the surface, or by the
logical volume entirely surrounded by this surface. The former is called a *border surface* while the latter is referred
to as the *skin surface*. This second type of surface is useful in situations where a volume is coded with a reflector
and is placed into many different mother volumes. A limitation is that the skin surface can only have one and
the same optical property for all of the enclosed volume's sides. The border surface is an ordered pair of physical
volumes, so in principle, the user can choose different optical properties for photons arriving from the reverse side
of the same interface. For the optical boundary process to use a border surface, the two volumes must have been
positioned with **G4PVPlacement**. The ordered combination can exist at many places in the simulation. When
the surface concept is not needed, and a perfectly smooth surface exists beteen two dielectic materials, the only
relevant property is the index of refraction, a quantity stored with the material, and no restriction exists on how
the volumes were positioned.

When an optical photon arrives at a boundary it is absorbed if the medium of the volume being left behind has no
index of refraction defined. A photon is also absorbed in case of a dielectric-dielectric polished or ground surface
when the medium about to be entered has no index of refraction. It is absorbed for backpainted surfaces when the
surface has no index of refraction. If the geometry boundary has a *border surface* this surface takes precedence,
otherwise the program checks for *skin surfaces*. The *skin surface* of the daughter volume is taken if a daughter
volume is entered else the program checks for a *skin surface* of the current volume. When the optical photon leaves
a volume without entering a daughter volume the *skin surface* of the current volume takes precedence over that
of the volume about to be entered.

The physical surface object also specifies which model the boundary process should use to simulate interactions
with that surface. In addition, the physical surface can have a material property table all its own. The usage of this
table allows all specular constants to be wavelength dependent. In case the surface is painted or wrapped (but not a
cladding), the table may include the thin layer's index of refraction. This allows the simulation of boundary effects
at the intersection between the medium and the surface layer, as well as the Lambertian reflection at the far side
of the thin layer. This occurs within the process itself and does not invoke the **G4Navigator**. Combinations of
surface finish properties, such as *polished* or *ground* and *front painted* or *back painted*, enumerate the different
situations which can be simulated.

When a photon arrives at a medium boundary its behavior depends on the nature of the two materials that join at
that boundary. Medium boundaries may be formed between two dielectric materials or a dielectric and a metal.
In the case of two dielectric materials, the photon can undergo total internal reflection, refraction or reflection,
depending on the photon's wavelength, angle of incidence, and the refractive indices on both sides of the boundary.
Furthermore, reflection and transmission probabilites are sensitive to the state of linear polarization. In the case of
an interface between a dielectric and a metal, the photon can be absorbed by the metal or reflected back into the
dielectric. If the photon is absorbed it can be detected according to the photoelectron efficiency of the metal.

As expressed in Maxwell's equations, Fresnel reflection and refraction are intertwined through their relative
probabilities of occurrence. Therefore neither of these processes, nor total internal reflection, are viewed as individual
processes deserving separate class implementation. Nonetheless, an attempt was made to adhere to the abstraction
of having independent processes by splitting the code into different methods where practicable.

One implementation of the **G4OpBoundaryProcess** class employs the UNIFIED model [A. Levin and C.
Moisan, A More Physical Approach to Model the Surface Treatment of Scintillation Counters and its Implementation
into DETECT, TRIUMF Preprint TRI-PP-96-64, Oct. 1996] of the DETECT program [G.F. Knoll, T.F. Knoll
and T.M. Henderson, Light Collection Scintillation Detector Composites for Neutron Detection, IEEE Trans.
Nucl. Sci., 35 (1988) 872.]. It applies to dielectric-dielectric interfaces and tries to provide a realistic simulation,
which deals with all aspects of surface finish and reflector coating. The surface may be assumed as smooth and
covered with a metallized coating representing a specular reflector with given reflection coefficient, or painted
with a diffuse reflecting material where Lambertian reflection occurs. The surfaces may or may not be in optical
contact with another component and most importantly, one may consider a surface to be made up of micro-facets
with normal vectors that follow given distributions around the nominal normal for the volume at the impact point.

For very rough surfaces, it is possible for the photon to inversely aim at the same surface again after reflection
of refraction and so multiple interactions with the boundary are possible within the process itself and without the
need for relocation by **G4Navigator**.

The UNIFIED model provides for a range of different reflection mechanisms. The specular lobe
constant represents the reflection probability about the normal of a micro facet. The specular spike constant, in
turn, illustrates the probability of reflection about the average surface normal. The diffuse lobe constant is for the
probability of internal Lambertian reflection, and finally the back-scatter spike constant is for the case of several
reflections within a deep groove with the ultimate result of exact back-scattering. The four probabilities must
add up to one, with the diffuse lobe constant being implicit. The reader may consult the reference for a thorough
description of the model.

The original GEANT3.21 implementation of this process is also available via the GLISUR methods flag. [GEANT
Detector Description and Simulation Tool, Application Software Group, Computing and Networks Division,
CERN, PHYS260-6 tp 260-7.].

The reflectivity off a metal surface can also be calculated by way of a complex index of refraction. Instead of
storing the REFLECTIVITY directly, the user stores the real part (REALRINDEX) and the imaginary part (IMAGINARYRINDEX)
as a function of photon energy separately in the G4MaterialPropertyTable. Geant4 then calculates
the reflectivity depending on the incident angle, photon energy, degree of TE and TM polarization, and this
complex refractive index.

The program defaults to the GLISUR model and *polished* surface finish when no specific model and surface
finish is specified by the user. In the case of a dielectric-metal interface, or when the GLISUR model is
specified, the only surface finish options available are polished or ground. For dielectric-metal surfaces, the
**G4OpBoundaryProcess** also defaults to unit reflectivity and zero detection efficiency. In cases where the
user specifies the UNIFIED model, but does not otherwise specify the model reflection probability
constants, the default becomes Lambertian reflection.

Martin Janecek and Bill Moses (Lawrence Berkeley National Laboratory) built an instrument for measuring the
angular reflectivity distribution inside of BGO crystals with common surface treatments and reflectors applied.
These results have been incorporate into the Geant4 code. A third class of reflection type besides dielectric-metal
and dielectric-dielectric is added: dielectric-LUT. The distributions have been converted to 21 look-up-tables
(LUT); so far for 1 scintillator material (BGO) x 3 surface treatments x 7 reflector materials. The modified code
allows the user to specify the surface treatment (rough-cut, chemically etched, or mechanically polished), the
attached reflector (Lumirror, Teflon, ESR film, Tyvek, or TiO2 paint), and the bonding type (air-coupled or glued).
The glue used is MeltMount, and the ESR film used is VM2000. Each LUT consists of measured angular distributions
with 4deg by 5deg resolution in theta and phi, respectively, for incidence angles from 0 to 90 degrees, in 1 degree
steps. The code might in the future be updated by adding more LUTs, for instance, for other scintillating materials
(such as LSO or NaI). To use these LUT the user has to download them from Geant4 Software Download and
set an environment variable, **G4REALSURFACEDATA**, to the directory of **geant4/data/RealSurface1.0**.
For details see: M. Janecek, W. W. Moses, IEEE Trans. Nucl. Sci. 57 (3) (2010) 964-970.




<!-- README.md ends here -->
