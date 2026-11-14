# Dataset for AI-Based Phishing URL Detection System

**Course:** CSSY4101 — B.Tech DS/SE  
**Selected approach:** Plan C — URL features + screenshot features + logo–domain matching

## 1. Dataset We Will Use

Our main dataset source will be **PhishIntention**, a research collection introduced with the 2022 USENIX Security paper *Inferring Phishing Intention via Webpage Appearance and Dynamics*.

Its publicly listed resources include:

- Approximately **25,000 benign webpages** and **25,000 phishing webpages** for experiments.
- A **reference-logo collection covering 277 brands**.
- A **logo-matching training set (3,061 samples)** and **test set (2,000 cropped logos)**.
- **3,049 misleading but legitimate webpages**, useful for investigating false warnings.

These are separate resources, not one ready-to-train CSV. We will inspect the downloaded files and select records with a valid URL, screenshot, and phishing/legitimate label. Brand information and approved domains may need to be checked or added for the selected examples.

## 2. Why We Chose PhishIntention

Our project does more than examine suspicious words in a URL. It also checks **whether the brand shown on a webpage matches the website's actual domain**.

For example, a page might show a PayPal logo but be hosted on an unrelated domain. That mismatch is a useful warning sign, although it does not automatically prove phishing.

PhishIntention is suitable because it provides screenshots, legitimate and phishing examples, and reference logos. Its misleading-legitimate collection is especially useful for checking **false positives**: legitimate pages sometimes show other companies' logos for login or payment services.

## 3. How We Plan to Use the Data

1. **Prepare records:** Organize selected URLs, screenshots, and labels (`0 = legitimate`, `1 = phishing`). Remove duplicates and check missing files.
2. **Extract URL features:** Calculate URL length, digits, dots, hyphens, and other simple lexical characteristics.
3. **Extract screenshot features:** Calculate basic image measurements such as brightness, color statistics, and edge density.
4. **Match logos to domains:** Use reference logos or an existing logo-recognition component to identify a brand, then compare the site's registered domain with a checked list of approved brand domains.
5. **Train models:** Combine the features and compare Logistic Regression, Random Forest, and XGBoost.
6. **Evaluate and demonstrate:** Measure precision, recall, F1-score, ROC-AUC, and especially **false-positive rate**. Connect the selected model to a local Flask service and a Chrome extension for testing.

For logo matching, we will distinguish **match**, **mismatch**, and **unknown**. An unrecognized logo must not be treated as proof that the page is safe.

## 4. Important Notes

- We are **not** planning to reproduce PhishIntention's full deep-learning and webpage-interaction system.
- We may begin with a small number of supported brands and expand if the results are reliable.
- We must verify that each selected URL really matches its screenshot and label.
- Training and testing should use separate websites, avoiding duplicate or nearly identical pages across both sets.
- Dataset downloads, availability, and reuse conditions should be checked before final selection.
- We will work from downloaded research data where possible rather than browsing live phishing sites on a personal computer.

## 5. Official Sources

- **PhishIntention datasets:** https://sites.google.com/view/phishintention/experiment-structure
- **Official code:** https://github.com/lindsey98/PhishIntention
- **Research paper:** https://www.usenix.org/conference/usenixsecurity22/presentation/liu-ruofan

**Project goal:** Build and evaluate a practical logo–domain-aware phishing detector, then demonstrate it through a locally installed Chrome extension.
