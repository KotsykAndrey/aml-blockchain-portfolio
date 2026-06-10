# Case 3 - KYC Document Forensics & Sanctions Screening
 
### Type: KYC Fraud Detection / Document Forensics / Sanctions Screening
 
### Date: June 2026
 
### Tools: PRADO (EU public document register), ICAO 9303 MRZ logic, PDF/EXIF metadata viewer (metadata2go.com), Google Maps (address verification)
 
## Overview
 
This case is a practical demonstration of the following skills:
1. KYC AI fraud prevention (deepfake, AI-generated images/videos/documents, synthetic identity)
2. KYC verification (passport, driver's licence, proof of address, and establishing SoF / SoW). Detection of: tampered passports, MRZ mismatches, forged proof of address, fake bank statements, and stolen identities
3. Sanctions screening (concept level; in a real case a tool such as world-check.com would be used)
4. Red flag identification
5. Writing alert disposition notes (documenting false/true positives, escalation to senior management, and RFIs to clients)
6. Mock SAR for suspicious activity, and a mock OFAC blocking report for a true-positive sanctions match
   
This case contains nine fictional fraud scenarios across different document types and jurisdictions. I split it into four parts that follow the real onboarding process.
This matters specifically for a crypto platform. Onboarding KYC is the only point where a real-world identity is tied to an account, and from there to on-chain activity.
A crypto withdrawal is irreversible and there is no chargeback, unlike a bank. If a fraudster opens an account with a fake or stolen identity and moves funds out, the money is unrecoverable.
An account opened on a synthetic identity is exactly what laundering and account-takeover theft rely on. Detecting fraudulent documents at onboarding is what prevents a fake identity from ever being attached to a wallet.
 
| # | Document | Country / Type | Fraud type | Decision |
|---|---|---|---|---|
| 1 | Passport | Italy | Tampered (MRZ mismatch) | Reject (+ SAR if prior funds) |
| 2 | Driver's licence | USA (New York) | Edited card / front–back mismatch | Reject (+ SAR if prior funds) |
| 3 | Selfie | - | Deepfake / AI-generated | Reject + SAR |
| 4 | Passport | Nigeria | Stolen identity | Reject + SAR |
| 5 | Utility bill | UK | Forged proof of address | Request more (+ SAR if refuses) |
| 6 | Bank statement | UAE bank | Fake (PDF metadata, arithmetic) | Reject (+ SAR if account active) |
| 7 | Passport | Ukraine | Synthetic identity | EDD + SAR |
| 8 | Passport | Saudi Arabia | Sanctions false positive | Close + document |
| 9 | Passport | Russia | Sanctions true positive | Freeze + SAR + OFAC report |
 
---
 
> **Disclaimer:** All scenarios are fictional and created only for educational and portfolio purposes. All passport images are specimens from the EU public document register. No real identities were used.
> (the source for each document is linked at the end). Mock SARs are fictional. All institution details are fictional.
> For passports I use official PRADO specimens; the driver's licence is an official public sample from the New York State DMV. For utility bills and bank statements, which have no
> safe specimens, I use schematic layouts that show the structure.
 
---
 
## KYC Fraud Methods
 
The nine documents map onto the main families of KYC fraud an analyst sees in crypto onboarding:
 
```mermaid
flowchart TD
    A["KYC Fraud"] --> B["Identity Fraud"]
    A --> C["Supporting Document Fraud"]
    A --> D["Profile Fraud"]
    A --> E["Sanctions Evasion"]
 
    B --> B1["Tampered document (edited passport / driver's licence)"]
    B --> B2["Deepfake / synthetic face (AI-generated)"]
    B --> B3["Stolen identity (real document, wrong person)"]
 
    C --> C1["Forged proof of address"]
    C --> C2["Fake bank statement / SOF"]
 
    D --> D1["Synthetic identity (fabricated person)"]
 
    E --> E1["False positive (common name collision)"]
    E --> E2["True positive (real sanctioned party)"]
 
    style A fill:#1A4A3C,color:#fff
    style B fill:#2A6E54,color:#fff
    style C fill:#2A6E54,color:#fff
    style D fill:#2A6E54,color:#fff
    style E fill:#8b0000,color:#fff
```
 
---
 
## Part 1 - Identity Documents
 
The first question in any onboarding is whether the person is who they claim to be. There are three ways this can fail (the document is edited, the face is fake, or the document is genuine but belongs to someone else).
 
> **Note on SAR thresholds at onboarding.** Several decisions below say "reject, and file a SAR if funds were already deposited." This reflects a real threshold question. For US MSBs the mandatory SAR trigger
> is a transaction conducted or attempted at or through the institution at or above the reporting threshold (31 CFR 1022.320). A fake document submitted at sign-up with no transaction and no funds does not
> automatically meet that bar - but many firms still file a *voluntary* SAR on attempted identity fraud, and firm policy may require it. So the pattern is: always reject and record internally; file a SAR when
> funds/transactions are present, when it fits a known fraud pattern, or where firm policy directs filing on attempted fraud.
 
### Document 1 - Italian Passport (Tampered MRZ)
 
**Document type:** Passport, Repubblica Italiana 

**Specimen reference:** PRADO ITA-AO-02005 - https://www.consilium.europa.eu/prado/en/ITA-AO-02005/index.html
 
![Italian passport (PRADO specimen)](images/doc-1-italy-passport.png)
 
*Specimen source: PRADO (Council of the EU), document ITA-AO-02005. The data shown (ROSSI MARIA, passport KK6000533) is the public PRADO specimen. The tampering in the scenario below is fictional, applied to these specimen details for the exercise.*

---

#### The scenario
 
A customer submits an Italian passport for ROSSI MARIA during onboarding. At first inspection the document appears correct. The layout, photo, and Italian text match the template. However, when I verify
the MRZ check digits, the calculation on the passport number fails. This indicates that the document number was altered after the passport was issued.

---

#### Step 1 - Visual inspection against the specimen
 
I compare the submitted passport against the genuine PRADO specimen, which is the reference standard for a real Italian passport. In a remote KYC review I work from a scan or a photo, so
I can only rely on features that are visible in an image. UV and physical magnification checks are possible only during an in-person review.

---

I check three features that are visible on a good scan:
 
**Facial photograph (biodata page)**
 
![Italian passport facial photograph (PRADO specimen)](images/doc-1-italy-passport-personaldata.png)
 
This screenshot shows the holder's photograph on the biodata page. On a genuine Italian passport the photograph is printed directly into the page surface during production, not attached on top, so it cannot be
replaced without damaging the page. I check the photograph for signs of substitution. I look for edges that appear added, a print texture that differs from the rest of the page, or a face that does not sit
naturally within the frame. A clean, evenly printed photograph that matches the surrounding page is what I expect on a genuine document.

---

**Personal data and optically variable device (hologram / OVD)**
 
![Italian passport biodata page with data, MRZ and OVD (PRADO specimen)](images/doc-1-italy-passport-hologram.png)
 
This screenshot shows the full biodata page: the surname (ROSSI), the given name (MARIA), the passport number (KK6000533), the dates, the two MRZ lines at the bottom, and the OVD (hologram) that overlaps the
photo. On a genuine Italian passport the personal data is laser-engraved, which gives it a specific texture and tone. Data that has been typed over or reprinted appears flat and often shows a slightly different
colour, so I confirm that the fonts, spacing, and field positions match the specimen. The OVD changes appearance when tilted. On a scan I cannot tilt it, but I can check that the overlay sits correctly over the
photo and that its edges look natural rather than digitally pasted. Forgers often damage or misalign this feature when they replace a photo.

---

**Laser-perforated numbering**
 
![Italian passport laser-perforated number KK6000533 (PRADO specimen)](images/doc-1-italy-passport-numbering.png)
 
The passport number (KK6000533) is laser-perforated through the pages. Each digit is physically punched through the paper, which cannot be reproduced by image editing. If the number on the scan appears
flat or printed rather than perforated, that is a forgery indicator. This number must also match the number encoded in the MRZ.

> Note: the Italian passport also carries UV features (fluorescent overprint, fibres, security thread, and a watermark). These are listed in PRADO, but I do not rely on them for a remote review because
> they can only be checked physically under UV light. I mention them for completeness, not as checks I perform on a scan.

---

#### Step 2 - MRZ verification and check digits
 
The Machine-Readable Zone (ICAO 9303, TD3 format) is two lines of 44 characters at the bottom of the biodata page. It carries built-in check digits that must validate mathematically.
This is the single most reliable technical check on a passport, because it requires no special equipment, only the algorithm.
 
```
Check-digit algorithm (ICAO 9303)
1. Map characters:  0-9 = face value · A=10, B=11 ... Z=35 · '<' = 0
2. Apply repeating weights:  7, 3, 1, 7, 3, 1 ...
3. Sum all (value × weight)
4. Check digit = sum mod 10
```
 
**Passport number check - worked example.**
 
The genuine specimen number is `KK6000533`. First let me confirm the genuine check digit, then show the tampered version.
 
```
Genuine - KK6000533
K  K  6  0  0  0  5  3  3
20 20 6  0  0  0  5  3  3     ← character values (K = 20)
7  3  1  7  3  1  7  3  1     ← weights
140 60 6  0  0  0  35 9  3    ← products
 
Sum = 253
253 mod 10 = 3
 
Correct check digit = 3   ← matches the genuine specimen MRZ (…533‹3›)
```
 
The genuine passport validates correctly with check digit `3`. Now consider the tampered version. The forger changed the last digit of the number to disguise the document (`KK6000538`) but left
the printed check digit as `3`.
 
```
Tampered - KK6000538 (printed check digit still 3)
K  K  6  0  0  0  5  3  8
20 20 6  0  0  0  5  3  8
7  3  1  7  3  1  7  3  1
140 60 6  0  0  0  35 9  8
 
Sum = 258
258 mod 10 = 8
 
Correct check digit = 8
MRZ prints          = 3     ← MISMATCH
```
 
The printed check digit is `3`, but for `KK6000538` it must be `8`. This does not happen by accident, because a valid passport always has matching check digits. A mismatch means that the document number
was altered after the passport was made, or that the MRZ was fabricated. This is strong evidence of tampering.

---

**Date of birth check, second example to show the method holds.**

The genuine specimen DOB is `901101` (1 November 1990). I first confirm the genuine check digit, then show a tampered version.
 
```
Genuine - 901101 (1 Nov 1990)
9  0  1  1  0  1
7  3  1  7  3  1
63 0  1  7  0  1
 
Sum = 72
72 mod 10 = 2
 
Correct check digit = 2   ← matches the genuine specimen MRZ (901101‹2›)
```
 
Now consider a tampered version. Suppose the forger changes the year of birth to make the holder appear five years younger (`951101`) but leaves the printed check digit as `2`.
 
```
Tampered - 951101 (1 Nov 1995, printed check digit still 2)
9  5  1  1  0  1
7  3  1  7  3  1
63 15 1  7  0  1
 
Sum = 87
87 mod 10 = 7
 
Correct check digit = 7
MRZ prints          = 2     ← MISMATCH
```
 
A genuine passport's DOB check digit for 901101 is `2`. If someone alters the date of birth, the check digit no longer matches. This is another independent tampering signal, exactly like the passport-number check.
 
---
 
#### Step 3 - Cross-reference
 
I compare every data point across the passport, the MRZ, and the application form.
 
| Field | Passport (visual) | MRZ | Application form |
|---|---|---|---|
| Surname | ROSSI | ROSSI | ROSSI |
| Given name | MARIA | MARIA | MARIA |
| Passport number | KK6000538 | KK6000538**3** (bad check) | KK6000538 |
| DOB | 01/11/1990 | 901101 | 01/11/1990 |
 
Here the names and the date of birth all agree. The problem is only the MRZ check digit on the altered passport number. In another common pattern the names would also disagree, for example a form name
that is a different name from the one on the passport rather than a spelling variant, which would be a second independent red flag.

The laser-perforated number is a further data point on the passport itself. In this scenario it still reads KK6000533, while the printed number and the MRZ read KK6000538. This internal disagreement confirms that the printed number and the MRZ were altered after issue.

---

#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| MRZ passport-number check digit does not validate | Document tampering | 🔴 CRITICAL |
| Laser-perforated number (KK6000533) does not match the printed number and MRZ (KK6000538) | Document integrity | 🔴 HIGH |
| (If found) given name on form ≠ name on passport | Identity mismatch | 🔴 HIGH |
| (If found) visual DOB ≠ MRZ DOB | Document integrity | 🔴 HIGH |
 
---
 
#### Decision & Action
 
❌ **REJECT.** Do not onboard. The action path is as follows.
 
- **Reject and issue an RFI.** Decline the document and request a clean re-submission of the original passport, together with a second independent identity document.
- **Verify.** Re-run sanctions and adverse-media screening on the verified identity, and flag the profile as HIGH RISK pending verification.
- **Escalate.** Route the case to a senior analyst with the findings if the re-submission also fails or if anything else is suspicious.
- **SAR.** File a SAR if funds were already deposited before this review, or if the case matches a pattern of similar fraudulent applications. A tampered document with no funds is normally a rejection
  and an internal fraud note. A tampered document attached to funds already on the platform is a SAR (see the note on attempted-onboarding SAR thresholds in Part 1).
- If the customer cannot produce a valid document, offboard and file a SAR.

---

#### Key learning
 
Check digits are the fastest way to detect a tampered passport. I first confirmed that the genuine number validates (`KK6000533` produces check digit 3), then showed how an altered number breaks the check. 
The laser-perforated number adds a second, independent check, because it cannot be altered on an existing passport: when the printed number and the MRZ read `KK6000538` but the perforation still reads `KK6000533`, 
the document contradicts itself. On a scan UV is not available, so the perforated number and the OVD over the photo are the visual features I rely on, together with the MRZ calculation.
 
---
 
### Document 2 - USA Driver's Licence (Edited Card / Front–Back Mismatch)
 
**Document type:** Driver's licence, New York State (USA)

**Sample reference:** NY DMV - Sample Photo Documents - https://dmv.ny.gov/driver-license/sample-photo-documents
 
![New York State driver's licence, front (NY DMV sample)](images/doc-2-usa-dl-front.png)
![New York State driver's licence, back (NY DMV sample)](images/doc-2-usa-dl-back.png)
 
*Sample source: New York State DMV official "Sample Photo Documents" page. Public reference document.*

---
 
#### Why a driver's licence is different from a passport
 
In the United States there is no national ID card, so the driver's licence is the primary identity document. This makes it the most forged ID in US-facing onboarding. Two things change compared with a passport.
 
- **There is no ICAO MRZ.** The machine-readable part is a PDF417 2D barcode on the back, which encodes the same data printed on the front.
- **There are two different numbers**, and they sit on different sides of the card. This gives a built-in front-to-back cross-check.
  
| Number | Where | What it is |
|---|---|---|
| **Client ID (CID)** | Front, 9 digits (e.g. `123 456 789`) | Identifies the person. Does not change on renewal. |
| **Document Number** | Back, 8–10 characters after "Doc #" (e.g. `ASD4567890`) | Identifies the card. Changes every time the card is reissued. |
 
There is also a card-type distinction that matters for compliance.
 
- **Standard** licence. Marked "NOT FOR FEDERAL PURPOSES" in top right corner. Since 7 May 2025 it cannot be used to board a domestic US flight or enter a federal building.
- **Enhanced (EDL)**. Has a US flag image and is REAL ID compliant.
- **REAL ID**. Has a star and is REAL ID compliant.

---

#### The scenario
 
A US customer submits the front and back of a New York driver's licence. The front appears clean, with a photo, the name (Marie Michelle Motorist), and CID `123 456 789`. However, two checks fail.
The data on the front does not match the PDF417 barcode on the back, and the Document Number format is wrong. This is the driver's-licence equivalent of an MRZ mismatch.
The human-readable side was edited, but the machine-readable side was not updated to match.

---

#### Step 1 - Visual inspection (front)
 
I compare the front against the official NY DMV sample:
 - Photo placement, fonts, and the NY State background and security print match the template
- "Class D" and the date format are correct for NY
- I check for editing signs around the name, DOB, and photo, such as mismatched fonts, uneven spacing, or a photo that sits incorrectly

---

#### Step 2 - Barcode (PDF417) cross-check, the key test
 
The barcode on the back is the machine-readable record of the card. It encodes the same data printed on the front. In a real KYC platform (Jumio, Onfido, Sumsub) the barcode is decoded automatically
and compared against the front, so the analyst sees a match or mismatch result, not the raw decoded data. I do not decode the barcode by hand, because the 2D code cannot be read by eye.
 
For this educational case the comparison below is illustrative. It shows what the platform would flag, not data I personally decoded.
 
| Field | Front (printed) | Back barcode (decoded) | Result |
|---|---|---|---|
| Name | MICHELLE, MARIE | MICHELLE, MARIE | ✅ OK |
| DOB | 10/31/1990 | 03/14/1988 | ❌ MISMATCH |
| ID | 123 456 789 | 123 456 789 | ✅ OK |
| Expiry | 10/31/2029 | 10/31/2029 | ✅ OK |
 
A genuine licence is produced in one process, so the front and the barcode always agree. A DOB that differs between the printed front and the encoded barcode means the front was altered after issue.
This is decisive evidence of tampering, following the same logic as a failed passport check digit.

---

#### Step 3 - Document Number format check
 
NY Document Numbers are an 8 to 10 character mix of letters and numbers. I check the format and the front-to-back relationship.
 - The Document Number is on the back ("Doc #"). If a submitted "Document Number" is presented as the 9-digit front number, the submitter has confused the CID with the Document Number,
    which suggests they do not actually hold the card.
- A Document Number that is the wrong length or character pattern for NY is a red flag.
  
---

#### Step 4 - Cross-reference
 
| Field | Front (visual) | Back barcode | Application form | Result |
|---|---|---|---|---|
| Name | Michelle, Marie | Michelle, Marie | Michelle, Marie | Match |
| DOB | 10/31/1990 | 03/14/1988 | 10/31/1990 | Mismatch |
| CID | 123 456 789 | 123 456 789 | 123 456 789 | Match |
 
The DOB on the front does not match the barcode. The customer also moved the year forward on the front (1988 to 1990), which is a common edit to defeat an age check or to match a stolen profile.
 
Separately, the card is a Standard licence, marked "Not for Federal Purposes". This is not a comparison field, but it is a limitation worth noting: the card is not REAL ID compliant and represents a weaker identity tier.

---

#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| Front DOB does not match the PDF417 barcode | Document tampering | 🔴 CRITICAL |
| Document Number wrong format / confused with CID | Document integrity | 🔴 HIGH |
| Editing signs around DOB / photo area | Tampering signal | 🔴 HIGH |
| Standard ("Not for Federal Purposes") card offered as full ID | Document-tier limitation | 🟡 MEDIUM |

---

#### Decision & Action
 
❌ **REJECT.** Do not onboard. The action path is as follows:
- **Reject and issue an RFI.** Request the original document and a second independent ID (a passport).
- **Verify.** Re-scan the barcode at higher quality to confirm that the mismatch is real and not caused by a poor image, and flag the profile as HIGH RISK.
- **Escalate.** Route the case to a senior analyst if the re-submission also fails or if other red flags appear.
- **SAR.** File a SAR if funds were already deposited, or if the edited licence is part of a wider fraud pattern (the same threshold logic as Document 1).
- If the customer cannot produce a consistent document, offboard and file a SAR.

---

#### Key learning
 
A US driver's licence has no MRZ, but the PDF417 barcode on the back must match the printed front. Comparing the two sides is the licence equivalent of validating a passport's check digit.
Knowing the difference between the CID and the Document Number is what allows me to identify a submitter who does not actually hold the card.
 
---
 
### Document 3 - Deepfake Selfie
 
**Document type:** Liveness selfie (face verification)

#### The scenario
 
The customer passes the document stage and submits a selfie for face matching and liveness. The selfie matches the document photo at a high score on commercial KYC platforms (Jumio, Onfido, Sumsub).
That high match is expected here, because the image was AI-generated from the document holder's own photo, not captured live from the real person. The attacker takes the photo on the genuine document, usually
stolen or purchased, and uses an AI model to produce a new image of that same face, which then passes the face-match check against the document. Several signals show that the selfie was AI-generated rather than
taken by a real person in front of the camera.

---

#### Step 1 - Visual indicators of AI generation
 
Modern GAN and diffusion faces are convincing, but they still leave indicators. I check for the following:
 
- **Hair edges.** Strands that blur or dissolve unnaturally into the background.
- **Ear and jaw symmetry.** AI faces are often too symmetrical, whereas real faces are not.
- **Eye reflections.** Real eyes reflect the room, while AI eyes often have mismatched or missing reflections.
- **Skin texture.** Too smooth, with no pores and no small imperfections.
- **Background.** The lighting on the face does not match the background lighting.
- **Accessories.** Glasses frames or earrings that are asymmetric or blend into the skin.

These visual indicators are useful, but they are weakening over time. Older models left clear artifacts, while current ai models can produce faces with none of the indicators listed above. 
For that reason I treat the visual layer as corroborating, not decisive. The weight of the decision rests on the layers that remain more reliable against a strong generator: the provenance metadata in Step 2, 
and the active or 3D liveness in Step 3.

---

#### Step 2 - Metadata analysis (EXIF and C2PA provenance)
 
A photograph taken on a real camera carries device metadata (EXIF): the make, the model, the lens, and the capture settings. An AI-generated image does not carry any of this, because no camera was involved.
What matters is not when the image was created, but whether the file shows a real capture device at all. One signal is the absence of any camera data, because every phone and camera records make, model, and lens data.
 
To demonstrate the difference, I compared a real photo taken on an iPhone with an image I generated in ChatGPT:
 
**Metadata of a genuine photo (taken on an iPhone)**
 
![Genuine iPhone photo metadata screenshot 1](images/doc-3-metadata-genuine-1.png)
![Genuine iPhone photo metadata screenshot 2](images/doc-3-metadata-genuine-2.png)
 
The file carries a full set of capture metadata: "Make: Apple", "Model: iPhone 14 Pro", the Lensmodel ("iPhone 14 Pro back triple camera 6.86mm f/1.78"), and the 
exposure settings ("FNumber: 1.8", "ExposureTime: 1/35", "ISO: 320", "FocalLength: 6.9 mm"). 
These fields are written by the camera at the moment of capture, and they are exactly what I expect on a genuine photo from a real device.

---

**Metadata of an AI-generated image (created in ChatGPT)**
 
![AI-generated image metadata screenshot 1](images/doc-3-metadata-ai-1.png)
![AI-generated image metadata screenshot 2](images/doc-3-metadata-ai-2.png)
![AI-generated image metadata screenshot 3](images/doc-3-metadata-ai-3.png)
 
The AI image carries no camera fields at all. There is no make, model, lens, or exposure data, because no camera produced it. Moreover, it carries a C2PA provenance manifest that openly identifies the file
as AI-generated. The `Actionssoftwareagentname` is `gpt-image`, the `Claim Generator Infoname` is `OpenAI Media Service API`, and the `Actionsdigitalsourcetype` value is a full IPTC link ending in `trainedAlgorithmicMedia`, which is the standard code for content produced by a generative model. The file is also C2PA-watermarked.

---

**The comparison**
 
```
Field                 Genuine photo (iPhone)              AI image (ChatGPT)
Make / Model      :   Apple / iPhone 14 Pro               none                       ← RED FLAG
Lensmodel         :   iPhone 14 Pro back triple camera    none                       ← RED FLAG
Exposure / ISO    :   f/1.8, 1/35, ISO 320                none                       ← RED FLAG
C2PA provenance   :   none                                gpt-image / OpenAI API     ← RED FLAG (declares AI)
digitalSourceType :   none                                trainedAlgorithmicMedia    ← RED FLAG (declares AI)
--- weaker signals, not red flags on their own ---
GPS / location    :   none                                none                       weak (geolocation can be off in settings)
Timestamp         :   capture time present                generation time present    not a flag (on-demand selfie is normal)
Color profile     :   Apple embedded profile              sRGB                       weak (most devices use sRGB)
File type         :   DNG (Apple ProRAW)                  PNG                        weak (PNG is also used for screenshots and on Android. DNG is just Apple's RAW format)
```
 
Two things expose the AI image. First, the absence of any camera capture data, because every phone and camera records make, model, and lens. Second, the presence of C2PA provenance that names the 
generator and marks the file as AI-generated. The first is a negative signal and the second is a positive one, and together they are decisive.

Metadata is not equally reliable in every case, and different AI tools behave differently. Mainstream generators such as ChatGPT now embed C2PA provenance that declares the image as AI-generated, but 
specialised fraud tools do the opposite: they strip all metadata, or spoof the camera fields to imitate a real device, and they never add C2PA. So in practice I read the metadata together with the visual 
indicators and the liveness result, rather than relying on a single layer. The presence of C2PA AI provenance is strong evidence that the image is generated. The absence of camera data is only suspicious, 
not proof, because a genuine photo can also lose its metadata after passing through a messaging app such as Telegram or WhatsApp.
 
If the metadata is missing or looks edited, I do not clear the image on that basis, and I do not reject it on that basis alone. I fall back to the layers an attacker cannot remove from the file: the visual AI
indicators, the liveness result, and the behavioural signals. Clean-looking metadata never clears a face by itself, and missing metadata never confirms fraud by itself. The decision comes from the combination.

> **Note:** The full metadata extractions for both files are attached in this repository: [genuine iPhone photo](attachments/doc-3-metadata-genuine.pdf) and [AI image](attachments/doc-3-metadata-ai.pdf).
> I produced both files myself to demonstrate the method. Neither file contains GPS or geolocation data.

---

#### Step 3 - Liveness detection and bypass techniques
 
How the attacker tries to bypass liveness check:
- **Virtual camera injection.** Software such as OBS or ManyCam feeds a pre-recorded or generated video into the KYC flow instead of a real webcam.
- **Replay attack.** A pre-recorded video of the AI face nodding and blinking.
- **Real-time deepfake.** AI generates the face live during the check.
How liveness detection catches it:
 - **Motion blur analysis.** Genuine head movement produces physically correct blur that matches the direction and speed of the movement. Deepfakes often show incorrect blur at the face edges,
   or "ghosting", where the face briefly doubles during a fast turn.
- **Depth and occlusion analysis.** Active liveness systems track how the face responds to small head movements. A flat photo or a replayed video does not produce correct parallax, so the depth
   relationship between facial features collapses. Some advanced systems also project random light patterns onto the face and verify that the skin responds correctly. A flat screen cannot reproduce this response.
- **Object occlusion test.** Passing a hand or an object in front of the face is one of the most reliable checks. A real face has a correct depth relationship with objects in front of it.
   A deepfake often loses face tracking at the moment of occlusion, so the real face briefly reappears, or artifacts appear at the boundary. A pre-recorded video simply shows the hand on top of the recording
   with no correct depth interaction.
- **3D depth liveness.** Unlike 2D (passive) liveness, infrared depth mapping cannot be faked with a flat screen or a replayed video, so it defeats most virtual-camera attacks.

---

#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| Multiple AI-generation artifacts in the selfie | AI-generated image | 🔴 HIGH |
| No camera capture metadata (no make, model, or lens) | Not produced by a camera | 🔴 HIGH |
| C2PA provenance marks the file as AI-generated (gpt-image, trainedAlgorithmicMedia) | AI provenance | 🔴 HIGH |
| Face match high but image provenance suspect | Synthetic identity | 🔴 HIGH |
| Liveness passed but background and lighting inconsistent | Possible injection attack | 🟡 MEDIUM |

---

#### Decision
 
❌ **REJECT + SAR.**
 
A deepfake selfie is not an innocent mistake. Someone deliberately generated a synthetic face to defeat identity verification. This is a clear fraud attempt, and the recommended practice is to file a SAR.
See the mock SAR below.
 
---
 
#### Mock SAR #1 - Deepfake / Synthetic Identity at Onboarding
 
> **Disclaimer:** Fictional SAR for educational purposes. Institution and subject details are fictional.
 
**Filing institution:** Clear Exchange Ltd. (VASP / MSB), FinCEN Registration No. XXXXXXX, Wilmington, DE 19801

**Date of report:** June 18, 2026

**Subject:** name as submitted, not verified; Application ID XXX-XXXXXX; no account opened, onboarding declined

**Prior SARs on subject:** none on file
 
**Narrative**
 
Clear Exchange Ltd. files this report to document an attempt to open an account using a genuine identity document together with an artificially generated (deepfake) selfie to pass identity verification.
No account was opened and no funds were received. The institution reports the attempt because the conduct shows a deliberate effort to defeat identity verification, which is
consistent with the early stage of account-takeover or money-laundering activity.
 
On June 17, 2026, the applicant submitted a passport and a liveness selfie through the institution's remote onboarding flow. The selfie passed the automated face-matching check
against the passport photograph, with a similarity score of 92 percent. During manual review, however, the analyst identified several indicators that the selfie was an artificially
generated image rather than a genuine photograph. The image showed unnatural rendering at the hair edges, facial features that were unusually symmetrical, and reflections in the eyes that
did not match a real environment. The file also carried no camera metadata of any kind. It contained no make, model, or lens information, which a photograph taken on a genuine device always records.
 
The liveness check was recorded as passed, but the captured video showed lighting and background characteristics that did not match a live capture. These characteristics are consistent
with a virtual-camera injection attack, in which pre-recorded or generated video is fed into the verification flow in place of a live webcam.
 
Taken together, the artificially generated facial image, the complete absence of camera metadata, and the signs of a virtual-camera injection indicate that the applicant attempted to create
a verified account under a fabricated or stolen identity. The institution considers this conduct consistent with the methods used to open accounts for the laundering of illicit funds or for fraud.
 
The institution declined the application on the same day and added the applicant's device fingerprint and the submitted image hash to its internal fraud watchlist. The submitted document, the selfie,
the associated metadata, and the device fingerprint have been retained and are available to law enforcement upon request. Any future application matching the retained device fingerprint or image hash
will be escalated immediately. The institution did not notify the applicant that this report was filed.
 
Point of contact: AML Compliance Officer, Clear Exchange Ltd.
 
**END OF MOCK SAR**
 
---

### Document 4 - Stolen Identity (Nigerian Passport)

**Document type:** Passport, Federal Republic of Nigeria

**Specimen reference:** PRADO NGA-AO-03001 - https://www.consilium.europa.eu/prado/en/NGA-AO-03001/index.html
 
![Nigerian passport (PRADO specimen)](images/doc-4-nigeria-passport.png)
 
*Specimen source: PRADO (Council of the EU), document NGA-AO-03001. Public reference document.*

#### The scenario
 
This is the hardest identity fraud to catch. The passport is completely genuine, with a real document, a real person, a valid MRZ, and correct check digits. The problem is that the person
submitting it is not the person it belongs to. It is a stolen, borrowed, or purchased identity.
Because the document itself is real, every document-level check passes. The fraud is only visible at the biometric and behavioural layer.

---

#### Step 1 - Document checks all pass

- MRZ valid, check digits correct
- Security features present
- Document not expired
- Name and DOB internally consistent
  
Nothing here fails. If I only checked the document, I would onboard a criminal.

---
 
#### Step 2 - Biometric mismatch (the main detection point)
 
The selfie does not match the passport photo. This is the primary detection layer for stolen identity. The detection has two forms: either the face-match score falls below threshold, or the same 
face matches a different applicant already on file, which means one face is being used with several documents.

---

#### Step 3 - Velocity and cross-platform signals
 
Registering on two platforms in one day is not suspicious on its own, because people compare exchanges and sign up on several at once. The signal is the combination:
- The same passport number is linked to a different face on another platform. One document cannot belong to two different people.
- The same passport appears across many platforms in a very short period of time (industrial identity farming).
- The same passport is used where the account was recently closed for fraud elsewhere.
- The device or IP is linked to previous fraud attempts.

---

#### Step 4 - Behavioural biometrics (backend signal)
 
This layer is not visible to the analyst directly. It runs on the backend (tools such as BioCatch or NeuroID) and produces a behavioural risk score that feeds into the onboarding decision.
It tracks how the form is filled, not only what is entered.
- **Typing speed.** A person who knows their own DOB and address types without hesitation. Slow, paused entry on personal fields suggests reading from someone else's document.
- **Copy-paste patterns.** Genuine users type their name and address manually. Fields pasted in seconds suggest an automated or pre-prepared submission.
- **Completion time.** The average KYC form takes 8 to 12 minutes. Completion in under 2 minutes with no corrections suggests a script.
- **Mouse and touchscreen movement.** Natural movement has acceleration and micro-jitter, whereas programmatic input is perfectly straight.
- **Hesitation patterns.** Someone filling another person's profile pauses on fields that require genuine personal knowledge.
  
No single behavioural signal confirms fraud. The system combines them into one score, and a high-risk score triggers a manual-review flag for the analyst.

---

#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| Selfie does not match passport photo | Biometric mismatch | 🔴 CRITICAL |
| Same passport linked to a different face on another platform | Identity abuse / document sharing | 🔴 HIGH |
| Same passport used across many platforms in a short period of time | Industrial identity farming | 🔴 HIGH |
| Same passport used after an account was recently closed for fraud elsewhere | Cross-platform fraud signal | 🔴 HIGH |
| Same face linked to other names/applications | Identity farming | 🔴 HIGH |
| High-risk behavioural-biometrics score (backend) | Behavioural mismatch | 🟡 MEDIUM |

---

#### Decision & Action
 
❌ **REJECT + SAR.**
 
A genuine document with a non-matching person points to a stolen identity. The real document-holder may be a victim of identity theft, or may be complicit and have sold their identity.
Either way the activity is suspicious. Unlike the no-funds document cases above, the deliberate use of someone else's genuine identity is a clear fraud, so I file a SAR.

---

#### Alert disposition note
 
> **Disclaimer:** Fictional internal note for educational purposes.
 
```
Case ID:        KYC-2026-00xxx
Applicant:      (passport holder name), onboarding
Review type:    Onboarding, identity verification
Risk rating:    High
Status:         REJECTED (fraud)
 
Trigger:        Genuine passport, but selfie does not match the document photo.
                Biometric mismatch on an authentic document.
 
Review conducted:
  - Document checks: MRZ valid, security features present, not expired
  - Biometric: face match below threshold vs document photo
  - Cross-platform: same passport linked to a different face elsewhere
  - No innocent explanation for an authentic document with a wrong face
 
Decision:       REJECT onboarding and FILE a SAR.
 
Rationale:      Use of a genuine third party's identity document by a different person is deliberate identity fraud.
                The true holder may be a victim, and the activity is reportable.
 
Analyst:        A. Kotsyk
Escalated to:   Senior AML Officer / MLRO
Next action:    Reject. Preserve device fingerprint and submitted image hash. File the SAR. Watch for re-attempts on the same identifiers.
Tipping off:    Applicant not told a SAR was filed (31 U.S.C. § 5318(g)(2)).
```
 
#### Key learning
 
When the document is real, document checks are not enough. The whole defence rests on biometric comparison and velocity checks. A perfect document is not a clean customer.
 
---

## Part 2 - Supporting Documents
 
Once identity is established, the next question is where the money comes from and whether the person actually lives where they say. These documents, utility bills and bank statements, are
the most commonly forged, because they are easy to edit in basic software.
 
### Document 5 - Forged UK Utility Bill
 
**Document type:** Utility bill (proof of address)
 
#### The scenario
 
The customer submits a UK electricity bill as proof of address. The address on the bill is 14 Kingsway, London WC2B 6LH. The document is forged. Someone used a template and typed in the customer's details,
or edited a real bill. The purpose of a proof of address is to confirm that the customer genuinely lives where they claim, which matters for jurisdiction, tax, and risk rating. A forged document breaks
that foundation, so I cannot accept it.

---

Schematic of the submitted bill (this is a layout, not a real document):
 
```
+--------------------------------------------------+
|  [LOGO]   BRITGRID ENERGY            Bill         |
|                                                   |
|  Daniel R. Whitfield          Account: 8839201    |
|  14 Kingsway, London                              |
|  WC2B 6LH                   Bill date: 15 Jan 2026|
|---------------------------------------------------|
|  Previous balance          £0.00                  |
|  Electricity (Jan)         £84.00                 |
|  --------------------------------                 |
|  Total due                 £84.00                 |
+--------------------------------------------------+
```
 
#### Step 1 - Font and layout consistency
 
A genuine utility bill is generated automatically by the utility's billing system, so every field uses the same fonts and alignment. On a forged bill I look for the following.
- The customer name in a slightly different font or weight from the rest, which suggests it was typed in later
- Numbers that are not aligned the way an automated system aligns them
- Uneven spacing where fields were edited

---

#### Step 2 - Logo quality
 
The logo is often taken from a web image at low resolution. A real bill uses a clean vector logo (SVG). JPEG artifacts or a blurred logo on an otherwise clean, sharp document is a red flag.

---

#### Step 3 - Metadata
 
If the file is a PDF, I check the metadata (see Document 6 for the full method). A utility bill "PDF" created in an image editor or a word processor is suspect.

---

#### Step 4 - Address verification
 
I check the address (14 Kingsway, London WC2B 6LH) in Google Maps and against postal data.
- Does the address exist?
- Does the postcode match the street?
- Is it residential or commercial?
  
On the residential versus commercial point, a commercial address is not automatically a red flag. There are legitimate situations, for example:
- **Mixed-use buildings.** Many city-centre buildings have shops on the ground floor and flats above, and people genuinely live there.
- **Live-work units.** These are common for self-employed people and freelancers.
- **Registered business address.** A sole trader may receive post at their business.
  
It becomes a red flag only in specific cases, such as:
- The address is a mailbox or virtual-office service, where post is forwarded and no one actually lives there.
- The address is a purely industrial or commercial building where residence is implausible, such as a warehouse or a shopping-centre unit.
- The address does not exist, or the postcode does not match the street.
  
I therefore treat a commercial address as a prompt to look closer, not as proof of fraud.

---

#### Step 5 - Date
 
Proof of address is normally accepted only if it was issued within the last 3 months (this depends on internal policies and procedures). An older bill is rejected on age alone.

---

#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| Customer name in different font from document body | Document integrity | 🔴 HIGH |
| Low-resolution / JPEG-artifact logo | Forgery signal | 🟡 MEDIUM |
| Address is a mailbox service or implausible for residence | Address concern | 🟡 MEDIUM |
| Bill older than 3 months | Stale document | 🟡 MEDIUM |

---

#### Decision & Action
 
⚠️ **REQUEST ADDITIONAL INFORMATION.** Do not onboard on this document.
 
Why request more rather than reject outright? A proof of address is a supporting document, and the correct first step when one appears forged is to give the customer a clear opportunity to provide a genuine,
independent one, which a legitimate customer can do easily. I do not accept the suspect bill, but I also do not yet assume crime. Further actions:
- Issue a Request for Information (RFI) asking for a second, independent proof of address.
- Do not verify the address until it is confirmed from an independent source.
- If the customer provides a clean document, proceed. The original is noted but set aside.
- If the customer refuses, delays, or sends another forged document, escalate and file a SAR, especially if funds are already on the platform.
- If the forgery is combined with other red flags, such as a suspect identity or suspicious funds, escalate immediately rather than only requesting more.

---

#### Mock RFI to the customer
 
> **Disclaimer:** Fictional RFI for educational purposes. Institution and customer details fictional.
> **Note:** Neutral, document-focused wording. There is no accusation and no mention of fraud, an investigation, or any report. This protects the process and avoids tipping off.
 
```
From:    Clear Exchange, Compliance / Onboarding
To:      Daniel R. Whitfield
Subject: Additional document required to verify your account
 
Hello,
 
Thank you for your recent submission. To complete the verification of your account, we need one additional proof of address.
The document you provided could not be accepted in its current form.
 
Please upload ONE of the following, issued within the last 3 months and clearly showing your full name and residential address:
  - Bank or credit-card statement
  - Utility bill (gas, electricity, or water)
  - Government or tax authority letter (e.g. council tax)
 
Requirements:
  - The document must be a full, original PDF or photo (no edits or cropping)
  - Name and address must match the details on your account
 
You can upload the document securely in your account under Settings, then Verification. If you have any questions, reply to this message.
 
Thank you,
Clear Exchange, Compliance Team
```
 
#### Key learning
 
Font consistency is the first and fastest check on a utility bill. Automated billing systems produce uniform documents, so any variation in the customer-specific fields is a strong sign of manual editing.
A commercial address is a reason to look closer, not an automatic red flag, because the context decides.
 
---

### Document 6 - Fake Bank Statement (UAE Bank)
 
**Document type:** Bank statement (source of funds)
 
#### The scenario
 
The customer is asked for a bank statement to support the source of funds for a large deposit. They submit a PDF that claims to be from a UAE bank. It is fabricated. This document has the largest set of red flags 
in the case, because a fake statement usually fails on several independent layers at once: the metadata, the running balance, the transaction patterns, and the visible consistency of the document itself.
 
---
 
The submitted statement is shown below, captured from the actual PDF file (which is also attached). It uses a single amount column, where income is + and spending is −:
 
![Fake Emirates First Bank statement screenshot](images/doc-6-bank-statement.png)
 
On the submitted PDF, the final row (28 March) is set in a different font and weight from the rest of the statement. The same row misspells "Incoming" as "Incomming" and drops the thousands separator that 
every other row uses, showing 25000 and 43000 rather than 25,000 and 43,000. An automated banking system renders every figure in one consistent font and one consistent number format, so a row that differs in font, spelling, and formatting was typed in by hand. This is the same font-consistency check used on the utility bill in Document 5, applied here to the exact row the forger edited: the large incoming transfer added 
to support the deposit.

---
 
#### Step 1 - PDF metadata (the fastest check)
 
A genuine bank statement is produced by the bank's core-banking system, and its PDF metadata reflects that. A fake one, edited in Word, reveals itself immediately.
 
The screenshot below shows a real metadata analysis from metadata2go.com. To demonstrate the technique, I created a test PDF in Microsoft Word, edited it once, and analysed it with the same tool 
I would use on a submitted document. The result shows two separate red flags in a single file.
 
![PDF metadata from metadata2go.com, the fake statement file](images/doc-6-metadata-word-creator.png)
 
> **Note:** The "author" field shows "Andrey" because this is a test file I created specifically to demonstrate the forensic method. In a real case the author field would show the name of the person who fabricated
> the document, which is itself a red flag. The key indicators are `creator_tool` and `producer`, both showing Microsoft® Word for Microsoft 365, not a banking system.
 
```
Metadata          Expected (genuine statement)               Found in this file
creator_tool  :   Core-banking PDF engine                    Microsoft Word for Microsoft 365       ← RED FLAG
producer      :   Adobe PDF Library / banking system         Microsoft Word for Microsoft 365       ← RED FLAG
creator       :   none (system-generated)                    Andrey (a personal windows username)   ← RED FLAG
create_date   :   matches the printed statement date         2026:06:09 17:30:02 (matches it)       ✓ consistent here
modify_date   :   identical to create_date                   2026:06:09 17:37:15 (7 minutes later)  ← RED FLAG (edited)
language      :   English or Arabic (UAE bank)               ru (Russian)                           ← minor indicator
```
 
A "bank statement" whose creator and producer are Microsoft Word was written in Word, not by a banking system. This is a 30-second check that catches the majority of fake financial documents. 
In this file the `create_date`, 2026:06:09 17:30:02, matches the generation date printed on the statement, 09.06.2026 at 17:30, so that particular check is consistent. A careful forger sets the printed date 
to match the file. The document is exposed by two other signals. First, the `creator` and `producer` are Microsoft Word for Microsoft 365, which a banking system would never produce. 
Second, the `modify_date`, 17:37:15, is seven minutes later than the `create_date`, which proves the file was opened and edited after it was created. A genuine statement is generated once and is never edited, so 
its create and modify dates are identical. Either signal on its own is enough to reject the document. The metadata carries smaller tells in the same direction: the author is a personal name rather than 
a banking system, the document language is set to Russian, and the `create_date` offset is +02:00 (Central European) rather than the +04:00 offset a UAE bank would use.
 
> **Note:** I created and edited the fake statement myself in Microsoft Word to demonstrate the method. The metadata report was generated automatically by metadata2go.com from that file, not written by me.
> Both are attached in this repository: the statement, [fake bank statement](attachments/doc-6-bank-statement.pdf), and its [metadata extraction](attachments/doc-6-bank-statement-metadata.pdf).
 
---
 
#### Step 2 - Balance arithmetic
 
This is the second most common mistake forgers make. They edit a number and forget to recompute the running balance. I verify the statement above.
 
```
17 Mar  Card payment  -300
Balance before: 16,000
16,000 - 300 = 15,700
Statement shows: 16,000     ← the 300 debit is not reflected
```
 
The running balance does not add up. The 17 March debit of AED 300 is not reflected, so the balance stays at 16,000 instead of falling to 15,700, and every later line is then 300 too high.
A real banking system never makes an arithmetic error, so this discrepancy is proof of manual manipulation.
 
---
 
#### Step 3 - Transaction patterns
 
Real accounts show varied, human spending. Fake statements appear too clean.
- Only round numbers, whereas real life includes amounts such as 47.32, not only 200
- No small everyday transactions, such as coffee, groceries, transport, and subscriptions
- A salary amount inconsistent with the stated annual income (here AED 10,000 per month, about AED 120,000 per year, does not match the AED 300,000 annual income stated on the application)
- A large unexplained "transfer in" just before the period ends, sized to support the deposit
  
---
 
#### Step 4 - IBAN and bank format
 
I check that the IBAN format is valid for the UAE (country code AE, correct length) and that the bank code maps to the bank named on the statement. 
A mismatch between the claimed bank and the IBAN's bank code is a red flag.
 
---
 
#### Red flags
 
| Red flag | Type | Severity |
|---|---|---|
| PDF creator = Microsoft Word | Document integrity | 🔴 CRITICAL |
| Modify date later than create date (file edited after creation) | Tampering evidence | 🔴 CRITICAL |
| Running balance does not add up (300 error) | Mathematical inconsistency | 🔴 CRITICAL |
| Edited row in a different font, with a misspelling and no thousands separator | Manual editing | 🔴 HIGH |
| Salary inconsistent with stated income | Profile mismatch | 🔴 HIGH |
| Only round-number transactions, no small spending | Unrealistic pattern | 🟡 MEDIUM |
| Large unexplained transfer-in sized to the deposit | Fabricated support | 🟡 MEDIUM |
 
---
 
#### Decision & Action
 
❌ **REJECT** the document, because it is forged. The action path is as follows:
- **Reject.** Inform the customer that the document cannot be accepted, using neutral language.
- **Request a verifiable replacement.** Ask for the original statement directly from the bank (certified), or through open-banking verification, which uses a direct API and cannot be faked.
- **Do not reveal** the specific forensic indicators found.
- **SAR.** File a SAR if the account is already active and the fake statement was provided to explain suspicious funds. If it is a first-time onboarding document with no funds yet, reject and request
     a genuine statement, but escalate for fraud review. If the account is active, freeze and file a SAR.
  
---
 
#### Mock letter to the customer
 
> **Disclaimer:** Fictional letter for educational purposes. Institution and customer details fictional.
> **Note:** Neutral wording. The customer is asked for a verifiable document through an independent channel. There is no mention of fraud, the specific forensic findings, an investigation, or a SAR, which
> avoids tipping off.
 
```
From:    Clear Exchange, Compliance
To:      Omar K. Haddad
Subject: Source of funds, additional verification needed
 
Hello,
 
As part of our standard checks, we need to verify the source of funds for recent activity on your account.
The statement you provided could not be accepted in its current form.
 
To continue, please provide ONE of the following:
 
  - A bank statement obtained directly through our open-banking verification link (fastest, connects securely to your bank)
  - An official statement issued directly by your bank, covering the last 3 months, showing your name, account number, and transactions
 
Please note that we cannot accept edited, cropped, or self-exported PDF files for this step.
Until this is completed, some account functions may remain limited.
 
You can start the open-banking verification in your account under Settings, then Source of Funds. If you have questions, reply to this message.
 
Thank you,
Clear Exchange, Compliance Team
```
 
#### Key learning
 
Bank-statement metadata is the fastest and most reliable forensic check. A real statement generated by a banking system will never show Microsoft Word as the creator, and its create and modify dates are identical 
because the file is never edited. A modify date later than the create date confirms a file that was changed after creation. The balance arithmetic is the second check, because forgers edit a number and forget to recompute the running balance across the page.
 
---
