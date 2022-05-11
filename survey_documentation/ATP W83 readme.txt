PEW RESEARCH CENTER
Wave 83 American Trends Panel 
Dates: Feb. 16 - Feb. 21, 2021
Mode: Web 
Sample: Subsample
Language: English and Spanish
N=10,121

***************************************************************************************************************************
NOTES

There was an error in the Spanish translation for the stem in question COVID_RESTRICTION. The question asks about policies related to address the coronavirus outbreak, but the word "policies" was incorrectly translated as "police" in the Spanish questionnaire. In the example below, the word "policías" should have been "políticas". 

English:
COVID_RESTRICTION   Thinking about policies in place in some areas to address the coronavirus outbreak, in general do you think each of the following is necessary or unnecessary?
 
Spanish:
COVID_RESTRICTION   Pensando en las policías que se han anunciado en algunas áreas para abordar el brote de coronavirus, en general, ¿considera que cada una de las siguientes medidas ha sido necesaria o innecesaria?

The error was noticed after data collection began. As a result of this error, 296 Spanish language respondents saw the incorrect wording. Those cases are identified in variable FLAG_W83 with value = 1 Incorrect COVID_RESTRICTION Spanish. After the error was corrected, 156 Spanish language respondents saw the correct wording. Those cases are identified in variable FLAG_W83 with value = 2 Correct COVID_RESTRICTION Spanish. All English language cases were unaffected. Those cases are identifed in variable FLAG_W83 with value = 3 English survey - not affected by translation error.
The 296 affected cases were coded as 98 = Translation error - do not include in analysis for COVID_RESTRICTIONa-h and are excluded from analysis. 


For a small number of respondents with high risk of identification, certain values have been randomly swapped with those of lower risk cases with similar characteristics.


***************************************************************************************************************************
WEIGHTS 

WEIGHT_W83 is the weight for the sample. Data for all Pew Research Center reports are analyzed using this weight.


***************************************************************************************************************************
Releases from this survey:

February 24, 2021 "More Americans now say academic concerns should be a top factor in deciding to reopen K-12 schools"
https://www.pewresearch.org/fact-tank/2021/02/24/more-americans-now-say-academic-concerns-should-be-a-top-factor-in-deciding-to-reopen-k-12-schools/
This release only uses WEIGHT_W83

March 5, 2021 "Growing Share of Americans Say They Plan To Get a COVID-19 Vaccine – or Already Have"
https://www.pewresearch.org/science/2021/03/05/growing-share-of-americans-say-they-plan-to-get-a-covid-19-vaccine-or-already-have/

March 9, 2021 "Black Americans stand out for their concern about COVID-19; 61% say they plan to get vaccinated or already have"
https://www.pewresearch.org/fact-tank/2021/03/09/black-americans-stand-out-for-their-concern-about-covid-19-61-say-they-plan-to-get-vaccinated-or-already-have/

March 16, 2021 "Many Americans continue to experience mental health difficulties as pandemic enters second year"
https://www.pewresearch.org/fact-tank/2021/03/16/many-americans-continue-to-experience-mental-health-difficulties-as-pandemic-enters-second-year/


***************************************************************************************************************************
SYNTAX

The Wave 83 dataset contains five coded variables:COVID_VAXCMB_W83, COVID_SELFSUM_W83, MHEALTH_W83, MHEALTH3_W83 and MFLAG_W83. SPSS syntax to compute these variables is included below.


COMPUTE COVID_VAXCMB_W83=$SYSMIS.
IF (COVID_SCI6E_W83=1 OR COVID_SCI6E_W83=2 OR COVID_VAXD_W83=1) COVID_VAXCMB_W83=1.
IF (COVID_SCI6E_W83=3 OR COVID_SCI6E_W83=4) COVID_VAXCMB_W83=2.
IF COVID_SCI6E_W83=99 COVID_VAXCMB_W83=3.
EXECUTE.
variable labels COVID_VAXCMB_W83 Thinking about vaccines to prevent COVID-19, do you think you will….
value labels COVID_VAXCMB_W83 1 'Definitely/Probably get vaccine, or already received at least one dose' 2 'Definitely/Probably NOT get vaccine' 3 'DK Ref'.

compute COVID_SELFSUM_W83=99.
IF COVID_SELF_B_W83=1 OR COVID_SELF_C_W83=1 OR COVID_SELF_E_W83=1 COVID_SELFSUM_W83=1.
IF (COVID_SELF_B_W83=2 AND COVID_SELF_C_W83=2 AND COVID_SELF_E_W83=2) COVID_SELFSUM_W83=2.
IF (COVID_SELF_B_W83=99 AND COVID_SELF_C_W83=99 AND COVID_SELF_E_W83=99) COVID_SELFSUM_W83=99.
EXECUTE.
variable labels COVID_SELFSUM_W83 Self-reported COVID.
value labels COVID_SELFSUM_W83 1 'Tested positive for COVID or antibodies, or pretty sure they had COVID' 2 'None of the self-reported Covid measures' 99 'DK, DK/No'. 

*setting all mental health measures to missing if refused or skipped.
missing values 
MH_TRACK_a_W83
MH_TRACK_b_W83
MH_TRACK_c_W83
MH_TRACK_d_W83
MH_TRACK_e_W83 
MH_TRACK_CV_W83 (99).

*computing additive index of mental health measures, excluding hopeful measure.
compute MHEALTH_W83 =  MH_TRACK_a_W83 +
MH_TRACK_b_W83 +
MH_TRACK_c_W83 +
MH_TRACK_e_W83 + MH_TRACK_CV_W83.
Execute.
variable labels MHEALTH_W83 'Simple additive index of 5 mental health measures'.

*creating three category index.
recode MHEALTH_W83 (5 thru 8=1)(9 thru 11=2)(12 thru 20=3) into MHEALTH3_W83.
value labels MHEALTH3_W83 1 'low - bottom 50%' 2 'medium - next quarter' 3 'high - top quarter'.
variable labels MHEALTH3_W83 'Responses to five mental health questions chunked into three groups'.

* creating a flag to indicate respondents who gave at least one response in highest category'.
compute MFLAG_W83=0.
if any (4,MH_TRACK_a_W83,
MH_TRACK_b_W83,
MH_TRACK_c_W83,
MH_TRACK_e_W83, MH_TRACK_CV_W83) MFLAG_W83=1.
EXECUTE.
VARIABLE LABELS MFLAG_W83 'Flag to indicate respondents who gave at least one response in highest category to MH_TRACK'.