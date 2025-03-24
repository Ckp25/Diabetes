# Diabetes Classification Project

## Project Overview
This project tackles the challenge of classifying individuals into three categories (healthy, prediabetic, and diabetic) using demographic and lifestyle data. The primary datasets contain over 70,000 entries with a significant class imbalance (84.2% healthy, 1.8% prediabetic, 13.9% diabetic).

## Data Characteristics
- **Severe imbalance**: Prediabetic class extremely underrepresented (1.8%)
- **Feature types**: Lifestyle factors, demographic information, and health indicators
- **Missing biomarkers**: No direct clinical measurements (HbA1c, glucose, insulin)
- **Fuzzy boundaries**: Inherent overlap between prediabetic and diabetic states

## Feature Engineering

### Basic Transformations
- **Normalization/Standardization**: Applied to numerical features to ensure consistent scale
- **Missing value handling**: Zero values in medical features treated as missing data

### Advanced Feature Creation
1. **Interaction Terms**
   - `BMI_X_Age`: Captures how BMI impact changes with age
   - `HighBP_X_HighChol`: Identifies compounding cardiovascular risk factors
   - `GenHlth_X_PhysHlth`: Represents interaction between general and physical health
   - `BMI_X_PhysActivity`: Measures how physical activity moderates BMI impact
   - `Age_X_HighBP`: Accounts for age-related blood pressure changes
   - `Income_X_Education`: Represents socioeconomic status
   - `Age_X_DiffWalk`: Captures mobility limitations with age

2. **Composite Indicators**
   - `health_bp_decay`: Continuous score capturing health deterioration
   - `metabolic_syndrome`: Binary indicator for multiple metabolic risk factors
   - `low_ses`: Binary indicator for low socioeconomic status
   - `condition_count`: Sum of reported health conditions
   - `health_cluster`: Categorical grouping of similar health profiles
   - `risk_score_mult`: Multiplicative risk score combining key factors

### Feature Selection & Importance
Top predictive features:
1. `risk_score_mult`: Highest predictive power across models
2. `Income_X_Education`: Strong negative correlation with diabetes
3. `BMI_X_Age`: Key interaction showing age-dependent obesity risk
4. `MentHlth`: Mental health emerged as surprisingly important
5. `condition_count`: Cumulative health condition burden

## Classification Results

### Binary Classification (Healthy vs. At-Risk)
- **Performance**: Strong results with 85% recall for non-healthy cases
- **AUC**: Consistently ~0.82 regardless of threshold adjustments
- **Best Algorithm**: CatBoost outperformed other approaches
- **Key Insight**: Binary classification works well even with imbalanced data

### Ternary Classification (Healthy, Prediabetic, Diabetic)
#### Direct Approach
- **Accuracy**: 66.48% with optimized thresholds
- **Prediabetic Precision**: Extremely poor (3%)
- **Key Challenge**: Fundamental overlap between classes in feature space

#### Hierarchical Two-Stage Approach
- **Stage 1**: Binary classifier (healthy vs. non-healthy)
  - 85% recall for non-healthy cases
- **Stage 2**: Distinguishing prediabetics from diabetics
  - Healthy class (0): 64% recall, 96% precision
  - Prediabetic class (1): 40% recall, 3% precision
  - Diabetic class (2): 54% recall, 38% precision
  - Overall accuracy: 62%

#### Balanced Ensemble Approach
- **Method**: Multiple models trained on balanced subsets with custom thresholds
- **Final Performance**:
  - Healthy class (0): 62% recall, 96% precision
  - Prediabetic class (1): 40% recall, 3% precision 
  - Diabetic class (2): 55% recall, 40% precision
  - Overall accuracy: 60%

### Risk Score Development
- **Formula**: `risk_score = prob_not_healthy * (0.5 + 0.5 * prob_diabetic)`
- **Class Separation**:
  - Healthy: Average score 0.37
  - Prediabetic: Average score 0.63
  - Diabetic: Average score 0.70

## What Worked

1. **Feature interactions**: The created interaction terms significantly improved model performance
2. **Hierarchical approach**: Breaking the problem into stages aligned with the natural progression of diabetes
3. **Continuous risk scoring**: Provided more nuanced assessment than discrete classification
4. **Ensemble methods**: Balanced training sets helped address extreme class imbalance
5. **Custom thresholds**: Optimizing decision boundaries for specific class recall targets

## What Failed

1. **Direct three-class classification**: Consistently poor performance in identifying prediabetics
2. **Complex neural networks**: Showed signs of overfitting without improving recall for minority classes
3. **Standard resampling techniques**: SMOTE and other simple resampling methods failed to address the fundamental class overlap
4. **Traditional metrics**: Accuracy proved misleading due to class imbalance
5. **Sharp classification boundaries**: Any attempt at hard classification between prediabetic and diabetic states

## Potential Scope for Improvement

### Technical Enhancements
1. **Explainable AI integration**: Implement SHAP or LIME to provide case-by-case feature importance
2. **Ordinal regression**: Explore specialized algorithms for ordered categorical outcomes
3. **Meta-learning**: Create a meta-model that learns when to trust each base model
4. **Active learning**: Implement a feedback loop where uncertain predictions guide data collection
5. **Longitudinal modeling**: Incorporate temporal data to predict disease progression

### Domain-Specific Extensions
1. **Personalized risk profiles**: Generate individual-specific risk factors and recommendations
2. **Regional adaptations**: Fine-tune models for different populations and demographics
3. **Intervention impact modeling**: Simulate effects of lifestyle changes on risk scores
4. **Integration with clinical biomarkers**: Create a hybrid model incorporating both lifestyle factors and clinical measurements
5. **Mobile application**: Develop an accessible tool for self-assessment and monitoring

### Research Directions
1. **Feature boundary analysis**: Further investigation of the fuzzy boundary between diabetes states
2. **Causal inference**: Move beyond correlation to identify causal relationships
3. **Transfer learning**: Leverage models from related health conditions
4. **Multi-task learning**: Simultaneously predict diabetes and related conditions
5. **Few-shot learning** approaches for the minority class

## Conclusion

The developed approach provides a practical solution for diabetes risk assessment despite the inherent challenges in the data. 
The continuous risk score acknowledges the spectrum nature of diabetes progression while providing actionable insights for screening and intervention.
While perfect separation of the three classes proved impossible with available features, the project successfully developed a clinically relevant tool for identifying at-risk individuals.
