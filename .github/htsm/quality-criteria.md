# Quality Criteria

A quality criterion is some requirement that defines what the product should be. Each item below can be thought of as a potential risk area. Quality criteria are subjective and multidimensional and often hidden or contradictory. For each item, determine if it is important to your project, then think how you would recognise if the product worked well or poorly in that regard.

Source: **Heuristic Test Strategy Model (HTSM)** by James Bach, v6.0.

---

## Capability — *Can it perform the required functions?*

- **Sufficiency**: the product possesses all the capabilities necessary to serve its purpose.
- **Correctness**: it is possible for the product to function as intended and produce acceptable output.

---

## Reliability — *Will it work well and resist failure in all required situations?*

- **Robustness**: the product continues to function over time without degradation, under reasonable conditions.
- **Error Handling**: the product resists failure in the case of bad data, is graceful when it fails, and recovers readily.
- **Data Integrity**: the data in the system is protected from loss or corruption.
- **Safety**: the product will not fail in such a way as to harm life or property.

---

## Usability — *How easy is it for a real user to use the product?*

- **Learnability**: the operation of the product can be rapidly mastered by the intended user.
- **Operability**: the product can be operated with minimum effort and fuss.
- **Accessibility**: the product meets relevant accessibility standards and works with O/S accessibility features.

---

## Charisma — *How appealing is the product?*

- **Aesthetics**: the product appeals to the senses.
- **Uniqueness**: the product is new or special in some way.
- **Entrancement**: users get hooked, have fun, are fully engaged when using the product.
- **Image**: the product projects the desired impression of quality.

---

## Security — *How well is the product protected against unauthorised use or intrusion?*

- **Authentication**: the ways in which the system verifies that a user is who they say they are.
- **Authorisation**: the rights that are granted to authenticated users at varying privilege levels.
- **Privacy**: the ways in which customer or employee data is protected from unauthorised people.
- **Security Holes**: the ways in which the system cannot enforce security (e.g. social engineering vulnerabilities).

---

## Scalability — *How well does the deployment of the product scale up or down?*

Consider both scaling up (more users, more data, more load) and scaling down (minimal environments, reduced resources).

---

## Compatibility — *How well does it work with external components and configurations?*

- **Application Compatibility**: the product works in conjunction with other software products.
- **Operating System Compatibility**: the product works with a particular operating system.
- **Hardware Compatibility**: the product works with particular hardware components and configurations.
- **Backward Compatibility**: the product works with earlier versions of itself.
- **Product Footprint**: the product doesn't unnecessarily hog memory, storage, or other system resources.

---

## Performance — *How speedy and responsive is it?*

Consider response time, throughput, and resource consumption under both typical and peak conditions.

---

## Installability — *How easily can it be installed onto its target platform(s)?*

- **System Requirements**: does the product recognise if some necessary component is missing or insufficient?
- **Configuration**: what parts of the system are affected by installation? Where are files and resources stored?
- **Uninstallation**: when the product is uninstalled, is it removed cleanly?
- **Upgrades/Patches**: can new modules or versions be added easily? Do they respect the existing configuration?
- **Administration**: is installation a process that is handled by special personnel, or on a special schedule?

---

## Development — *How well can we create, test, and modify it?*

- **Supportability**: how economical will it be to provide support to users of the product?
- **Testability**: how effectively can the product be tested?
- **Maintainability**: how economical is it to build, fix, or enhance the product?
- **Portability**: how economical will it be to port or reuse the technology elsewhere?
- **Localisability**: how economical will it be to adapt the product for other places?
