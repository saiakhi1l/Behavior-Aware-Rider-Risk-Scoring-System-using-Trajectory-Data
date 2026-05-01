 Behavior-Aware Rider Risk Scoring System using Trajectory Data

Overview
This project builds a rider risk scoring system using GPS trajectory data.  
Raw coordinate sequences are transformed into behavioral features such as speed, acceleration, and stop patterns.  
Anomaly detection is then used to identify unusual riding behavior and assign a continuous risk score.



Problem Statement
Identify potentially unsafe riding behavior using trajectory data in the absence of labeled “risky” or “safe” examples.

 Approach

 1. Data Processing
- Parsed GPS trajectory (polyline) data  
- Removed empty and invalid trips  
- Filtered unrealistic values (extreme speed and acceleration)  
- Removed idle or near-zero movement trips  

2. Feature Engineering
Derived behavioral features from raw coordinates:
- Average speed  
- Maximum speed  
- Average acceleration  
- Speed variation (max - avg)  
- Speed consistency  
- Stop rate (proportion of near-zero movement)  
- Hour of day  

 3. Modeling
- Used **Isolation Forest** for anomaly detection  
- Used **Local Outlier Factor (LOF)** for comparison  
- Observed ~92% agreement between both methods  

 4. Risk Scoring
- Converted anomaly scores into a normalized range (0–1)  
- Adjusted scores using stop rate to reduce bias from traffic-heavy trips  
- Final output is a continuous risk score instead of binary classification  



- ~1% of trips identified as high-risk  
- High-risk trips typically show:
  - High speed  
  - High acceleration  
  - Continuous movement (low stop rate)  

- Reduced false positives caused by traffic conditions using stop-rate adjustment  

Sample Output

| avg_speed | max_speed | avg_accel | stop_rate | risk_score |
|----------|----------|-----------|-----------|------------|
| 28.4     | 34.8     | 7.13      | 0.00      | 0.87       |
| 37.3     | 39.7     | 2.77      | 0.00      | 0.84       |



 Key Learnings

- Raw trajectory data must be transformed into meaningful behavioral features  
- Anomaly detection can identify unusual patterns but requires domain adjustments  
- Traffic-heavy patterns can be misclassified without proper feature design  
- Continuous scoring provides more flexibility than binary classification  

Limitations

- No labeled ground truth for “unsafe” behavior  
- Risk score is based on anomaly patterns, not verified incidents  
- No geographic context (e.g., road type, traffic rules)  
