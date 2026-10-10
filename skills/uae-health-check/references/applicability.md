# Which requirements apply

Use the scope answers to pick requirements from the library. When in doubt, include the requirement and let the engineer mark it Not applicable with a reason. A missed requirement costs more than an extra line.

## Who regulates the customer?

| Customer or deployment | Regulator | Library files |
|---|---|---|
| Facility licensed in Abu Dhabi, Al Ain or Al Dhafra | DoH | federal.md + doh-abu-dhabi.md |
| Facility licensed in Dubai (outside Dubai Healthcare City) | DHA | federal.md + dha-dubai.md |
| Facility in Sharjah, Ajman, Umm Al Quwain, Ras Al Khaimah or Fujairah, or an EHS/MOHAP facility | MOHAP | federal.md + mohap.md |
| Product sold into several emirates | All of the above that apply | Combine, and keep the stricter rule where they differ |
| Facility in Dubai Healthcare City | Dubai Healthcare City Authority (DHCA) | **Not covered by this library.** Apply federal.md and ask the customer for DHCA's rules. |

Federal rules (federal.md) apply everywhere in the UAE, including free zones (FED-01).

## Is the system built for a facility or sold as a product?

- **Built for or run by a facility:** the facility is the regulated entity. Requirements apply to the system and to the facility's processes.
- **Sold as a product** (EMR, app, platform, device software): the vendor is also in scope.
- **Built, hosted or supported by an outside company for a facility** (bespoke build, managed service): the builder is an external technology provider too.

In Abu Dhabi, both of the last two fall under the ADHICS "Service Provider" criterion (DOH-01). Add SP to the tier and apply every control tagged SP. If it's unclear, include SP and record it as an assumption to confirm.

## ADHICS tier (Abu Dhabi only)

Pick from DOH-01:

| Tier | Who |
|---|---|
| Basic | All in-scope entities |
| + Transitional | Hospitals with 1–20 beds, and all centres (diagnostic, dialysis, IVF, rehabilitation) |
| + Advanced | Hospitals with 21+ beds, Malaffi, insurers and TPAs |
| Service Provider | External technology providers: EMR, app and device vendors |

Include an item if any control inside it is tagged for the chosen tier(s). In the requirement text, name the specific controls that apply, for example "CO 6.3 (Basic) applies; CO 6.1 is Advanced only".

## Base sets

| Regulator | Base set | Note |
|---|---|---|
| Federal (every health ICT system in the UAE) | FED-02 to FED-09, FED-11 to FED-18, FED-25 | FED-01, FED-20 and FED-35 are scope or penalty provisions. Record them with status **Informational**. |
| DoH (Abu Dhabi) | DOH-01 to DOH-21, filtered by tier | DOH-22 is a status note. Record it as Informational. |
| DHA (Dubai) | DHA-05 to DHA-16 and DHA-23 | Security, consent and access, data sharing, transfers, breach, retention |
| MOHAP | MOH-03 and MOH-04 | Only when the system connects to Riayati. Otherwise apply only the add-ons below. |

## Add-ons by feature or situation

| If the system… | Add |
|---|---|
| Sends any health data outside the UAE, or uses an offshore service that receives it (analytics, error tracking, email/SMS, video, LLM APIs, support tools) | FED-21 to FED-24, DOH-07, DHA-15 (whichever emirates apply) |
| Holds personal data that isn't health data (staff, marketing, billing contacts) | FED-26, FED-27 |
| Connects to the national Central System or national unified medical record (for example Riayati). Ask if unsure: Malaffi and NABIDH are emirate HIEs, not the federal Central System. | FED-10, FED-19 |
| Is operated by, or for, a government entity or a designated critical-infrastructure entity | FED-33, FED-34 |
| Has AI features of any kind | FED-36, plus DOH-30 to DOH-33 (Abu Dhabi) and DHA-17, DHA-18 (Dubai) |
| Uses AI or software to diagnose, treat, monitor or drive clinical decisions, or is part of a medical device | FED-28 to FED-32 (Emirates Drug Establishment approval) |
| Offers telehealth or remote care | MOH-05 to MOH-09 everywhere: they come from federal Cabinet Resolution 40/2019, so record their regulator as "Federal". Add DOH-34, DOH-35 (Abu Dhabi) and DHA-19 to DHA-22 (Dubai). |
| Is used by a facility that must connect to the HIE | Abu Dhabi: DOH-23 to DOH-29 (Malaffi, Sahatna). Dubai: DHA-01 to DHA-04 (NABIDH). MOHAP: MOH-01 to MOH-04 (Riayati). |
| Holds health data in the source code repository, CI logs or build artefacts | Treat the git host and CI region as storage under FED-04, DOH-07 and DHA-15 |
| Handles insurance claims | CLM-01 (Abu Dhabi), CLM-02 (Dubai) |
| Includes medical devices or connected (IoMT) equipment | DOH-21 |

## Always ask: team location and remote access

ADHICS (DOH-07, DOH-11), DHA remote access (DHA-11) and the DoH Responsible AI Standard (DOH-32) restrict who may access or develop systems holding health data from outside the UAE. Include this as a **Blocker-priority question** whenever DoH or DHA applies.

It matters most for teams with offshore engineering or support. This is a reading of the regulators' text; a DoH or DHA exemption, or UAE-resident operations, may resolve it. Flag it; don't decide it.

## Items that are partly unconfirmed

Some items are confirmed in their main point but unconfirmed in a detail. FED-28, for example, is unconfirmed on whether software without hardware is covered. Keep the confirmed part as the requirement, and add the unconfirmed detail as a question in the same item. Set the status to Needs confirmation until the question is answered.

## Regulator value in the register

Use `Federal`, `MOHAP`, `DoH`, `DHA` or `Internal` (for house-rules items). MOH-05 to MOH-09 use `Federal` when applied outside MOHAP facilities.
