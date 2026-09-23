# icu-risk-warning-system
Early ICU Risk Warning System Based on Multiple Imputation and Tree Models

_Developed as part of BMI5101 coursework at NUS._

## Project Status
🚧 Ongoing

PROBLEM

When a patient is in the ICU, doctors watch numbers: heart rate, blood pressure, oxygen level, blood tests, and more. If these numbers start trending badly, that's often a sign the patient is deteriorating — sometimes hours before a nurse or doctor notices it themselves.
This is exactly the kind of pattern a computer model can be trained to catch early. Feed it a patient's numbers over time, and it can output a "risk score" updated continuously as new measurements come in.
The catch: ICU data might be often missing values. A blood test might be taken every 6 hours, not every minute. So at any given moment, most of the numbers a model would want are simply not there yet. This is called missing data, and it's the single biggest practical obstacle to real-time ICU prediction.

THE GAP ADDRESSED

To summarize the literature review we have done:
Many of the existing works have tried to fill missing values using Single-imputation and have explicitly stated it couldn’t be trusted 100% as it is a single guess value.
Some papers have neglected the missing records and some papers like RealMIP have implemented Multiple Imputation. However, the issues as stated in RealMIP research paper is that, though there could be multiple guessed values there’s no measure to highlight the distinction between observed and imputed value, which will act as a risk-score and help the doctors/nurses in decision-making.
So, we are trying to fill missing values using multiple computation as well as come up with a measure that says how much these values disagree with each other and come up with a confidence score.

IMPLEMENTATION

Collect Data

Fill missing values using python’s MICE
Compute risk scores for each imputed value using XGBoost
Pool (ensemble) them using techniques like Rubin’s rule  to see how much each score differs from one another and produce a single confidence score.
Can visualize it in a dashboard if possible.
