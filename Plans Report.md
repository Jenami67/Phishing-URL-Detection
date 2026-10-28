# Evaluation of Visual Feature-Based Approaches for AI Phishing Website Detection

## 1. Introduction

Phishing websites are designed to deceive users by imitating legitimate online services and requesting sensitive information such as passwords, usernames, and banking details. Detecting these websites can be challenging because attackers may use ordinary-looking URLs and create webpages that closely resemble trusted platforms.

Machine learning can help identify phishing websites by analyzing URL characteristics and webpage appearance. For the proposed AI-Based Phishing URL Detection System, three approaches are considered:

- **Plan A:** Phishing detection using lexical URL features only.
- **Plan B:** Phishing detection using URL features combined with basic screenshot features.
- **Plan C:** Phishing detection using URL features, screenshot features, and logo–domain matching.

These approaches offer different levels of functionality and complexity. The objective is to determine which provides the best detection performance while remaining suitable for implementation in a final-year project.

## 2. Plan A: Lexical URL-Based Detection

Plan A uses characteristics extracted directly from a website's URL. These include URL length, number of dots, special characters, subdomains, and suspicious words.

For example, consider this fictional phishing URL:

`https://paypal-login-secure.example/verify/account`

Your program extracts numerical features:

| Extracted feature                 | Example value |   |
| --------------------------------- | ------------- | - |
| URL length                        | 51            |   |
| Number of dots                    | 1             |   |
| Number of hyphens                 | 2             |   |
| Contains HTTPS                    | 1             |   |
| Contains suspicious word `login`  | 1             |   |
| Contains suspicious word `verify` | 1             |   |
|                                   |               |   |

The extracted information is converted into numerical features and used to train machine learning classifiers such as Logistic Regression, Random Forest, and XGBoost.

The main advantage of this approach is its simplicity and speed. It does not require opening a webpage or capturing screenshots, making data processing and prediction relatively fast.

However, URL-based detection has limitations. Some phishing websites use short and ordinary-looking URLs, while legitimate websites may contain complicated addresses with many parameters. As a result, URL characteristics alone may not always provide sufficient information for accurate classification.

This Plan will serve as the baseline against which more advanced approaches can be evaluated.

## 3. Plan B: Combining URL and Basic Visual Features

Plan B extends URL-based detection by including numerical features extracted from webpage screenshots.

```text
[
    51,       # URL length
    2,        # Number of hyphens
    185,      # Average brightness
    160,      # Average red channel
    175,      # Average green channel
    190,      # Average blue channel
    420.5,    # Color variance
    0.38,     # Dominant color ratio
    0.12,     # Edge density
    0.31,     # Text-region ratio
    0.18      # White-space ratio
]
```

These visual features may include average brightness, RGB color distribution, and edge density. Each measurement represents a property of the webpage's appearance and can be calculated using Python image processing libraries.

The visual measurements are combined with lexical URL features into a single feature vector. This allows traditional machine learning classifiers to analyze both types of information without requiring a separate deep learning architecture. This Method is called Early Feature Fusion.

The main advantage of Plan B is that it introduces visual information while keeping the model structure relatively simple. It also provides a clear experimental question: whether adding screenshot features improves phishing detection compared with using URL features alone.

However, Plan B has an important limitation. Basic visual measurements do not directly recognize company logos, login forms, or brand impersonation. Two unrelated webpages may have similar colors and brightness, while a phishing website may look nearly identical to the legitimate website it copies.

Therefore, Plan B provides a practical foundation, but its visual features may not capture some of the most important characteristics of phishing attacks.

## 4. Plan C: Logo–Domain Matching

Plan C extends the previous approaches by introducing logo recognition and domain verification.

The main idea is to identify the organization represented by a webpage and compare it with the website's actual domain.

For example, if a webpage displays the PayPal logo but operates under an unrelated domain, the system may identify a possible phishing attempt.

Unlike basic visual measurements, logo–domain matching examines the relationship between the webpage's claimed identity and its web address. This provides information that is more directly connected to brand impersonation.

### Implementation Possibilities

For the proposed project, Plan C does not necessarily require training a new image recognition model. A simplified implementation could use an existing logo recognition model or image matching techniques.

The process would involve:

1. Collecting reference logos and authorized domains for a selected group of well-known brands.
2. Capturing a webpage screenshot and identifying any recognized logos.
3. Extracting the website's actual registered domain.
4. Checking whether the recognized brand and domain correspond.
5. Adding the result as an additional feature for the phishing classifier.

For example, the system could use features indicating whether a known logo was detected and whether its domain matched the recognized brand. Cases where no reliable logo is identified would be treated as unknown rather than automatically legitimate.

### Main Challenges

Although Plan C is technically achievable, several issues must be considered.

**Logo Recognition:** Logos may appear in different sizes, colors, and positions. Basic image matching might fail when a logo is modified. Pretrained recognition models may reduce this problem, but their reliability still requires testing.

**False Positives:** A legitimate website may display another company's logo for authentication, advertising, or payment services. Therefore, a logo–domain mismatch is not always evidence of phishing.

**Unknown Brands:** A recognition system can only identify brands supported by its reference database or recognition model. Websites representing unfamiliar organizations may not produce useful logo information.

**Attackers may slightly modify**, resize, recolor, or distort a legitimate logo to evade recognition. These small changes can reduce logo-detection performance and cause the system to miss phishing webpages.

**Dataset Preparation:** Evaluating Plan C requires suitable screenshots, website labels, and information about recognized brands. These requirements may increase the effort needed for dataset preparation.

## 5. Proposed Implementation and Evaluation

The proposed system will be developed using Python, scikit-learn, XGBoost, pandas, NumPy, Flask, and suitable image processing libraries.

Data collection will focus on obtaining labeled phishing and legitimate websites with matching URLs and screenshots. The UCI Phishing Websites dataset can support initial classification experiments, but it does not contain the paired screenshots required for Plan B and Plan C [3]. Additional sources, such as the Phishpedia benchmark, will therefore be considered.

After data preparation, the experiments will compare the three feature configurations:

| Configuration | Features                                                           |
| ------------- | ------------------------------------------------------------------ |
| Model A       | Lexical URL features                                               |
| Model B       | URL features + basic screenshot features                           |
| Model C       | URL features + screenshot features + logo–domain matching features |

Logistic Regression, Random Forest, and XGBoost will be trained for each configuration, producing nine model experiments.

The models will be evaluated using accuracy, precision, recall, F1-score, and ROC-AUC. False positives is most important.

For a fair comparison, the experiments will use the same testing records. Plan C will also require additional testing on websites containing known brand logos, including legitimate pages that display third-party logos.

The selected model will be integrated into a Chrome extension through a prediction service. The extension is experimental and will not be published on the Chrome Web Store. It will be installed locally in Chrome using Developer Mode and tested in local environment. The extension will collect the required website information, request a prediction, and display a warning when a webpage is classified as suspicious.

## 6. Conclusion and Recommendation

The three approaches provide a logical development path for the proposed phishing detection system.

Plan A offers a simple and efficient baseline but is limited to information available in the URL. Plan B adds basic visual information and provides a manageable way to evaluate the benefit of combining URL and screenshot features.

Plan C introduces a more meaningful type of visual analysis by examining whether the identity presented on a webpage matches its actual domain. Although this approach introduces additional challenges in logo recognition, dataset preparation, and false-positive control, its implementation is achievable using existing tools and pretrained models.

For this final-year project, **Plan B is recommended as the main system, with Plan C included as an advanced experimental enhancement**. This approach will allow the project to establish a working phishing detection system while investigating whether logo–domain matching provides an additional improvement.

The final decision should be based on experimental results rather than implementation complexity alone. If Plan C improves detection without causing excessive false warnings, it may become the preferred model for Chrome extension integration.

## References

[1] B. van Dooremaal, P. Burda, L. Allodi, and N. Zannone, "Combining Text and Visual Features to Improve the Identification of Cloned Webpages for Early Phishing Detection," *Proceedings of the 16th International Conference on Availability, Reliability and Security (ARES)*, 2021.

[https://doi.org/10.1145/3465481.3470112](https://doi.org/10.1145/3465481.3470112)

[2] Y. Lin et al., "Phishpedia: A Hybrid Deep Learning Based Approach to Visually Identify Phishing Webpages," *30th USENIX Security Symposium*, pp. 3793–3810, 2021.

[https://www.usenix.org/conference/usenixsecurity21/presentation/lin](https://www.usenix.org/conference/usenixsecurity21/presentation/lin)

[3] R. Mohammad and L. McCluskey, "Phishing Websites," *UCI Machine Learning Repository*, 2015.

[https://archive.ics.uci.edu/dataset/327/phishing](https://archive.ics.uci.edu/dataset/327/phishing)

[4] Y. Lin et al., "Phishpedia: Official Implementation," GitHub Repository.

[https://github.com/lindsey98/Phishpedia](https://github.com/lindsey98/Phishpedia)
