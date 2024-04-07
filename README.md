This repository contains the prompts used in the GPT experiments for *"Reflectance Estimation for Proximity Sensing by Vision-Language Models: Utilizing Distributional Semantics for Low-Level Cognition in Robotics"*.

<br><br>

# Abstract of the paper

```
While large language models (LLMs) and vision-language models (VLMs) have been increasingly used in robotics for problems requiring high-level cognition, their use for low-level cognition, such as interpreting sensor information, has not been sufficiently explored. In robotic grasping, proximity sensors are used to adjust the grasping pose before contact but the reflectance of the target objects significantly impacts the distance measured by the sensors.Therefore, estimating the reflectance of objects is crucial for successful grasping, which requires interpreting sensing information. Since LLMs embed human knowledge by keeping the relation between words as postulated by distributional semantics, it may be possible that LLMs can estimate reflectance from only object names without using any image input.Moreover, as VLMs are trained with image-text pairs, it may be possible that the latent structure of language positively affects the image-based reflectance estimation.In this paper, we verify that 1) LLMs such as GPT-3.5 and GPT-4 can estimate an object's reflectance using only text as input; and 2) VLMs such as CLIP can increase their generalization capabilities and, consequently, achieve a higher accuracy in reflectance estimation from images. We evaluated the accuracy in reflectance estimation for unseen objects using LLMs with text input and VLMs with image input and compared them to image-only methods such as ResNet. As a result, GPT-4 achieved a mean error of 14.7%, lower than ResNet, even with the input being only text. Moreover, while CLIP achieved the lowest mean error of 11.8%, GPT-3.5 obtained 19.9%, a competitive result when compared to the image-only ResNet which achieved 17.8%. The results suggest that the distributional semantics in LLMs and VLMs increases their generalization capabilities and that the knowledge acquired by VLMs is positively affected by the latent structure of language.
```

<br><br>

# Prompt


```
You are an expert in material properties. Your task is to determine the infrared reflectance of a given object. 
Please provide a single, reasonable value from the estimated range of infrared reflectance for each object. If your prediction is not conclusive, please output an approximate value as a guess. Note that the reflectivity of plastic bottles varies depending on their contents and labels. The greater the divergence of the refractive indices, the stronger the reflection.

Here are some example objects and their reflectance for reference:

dressing bottles [plastic, orange] : 0.326
dressing bottles [plastic, brown] : 0.327
dressing bottles [plastic, grean] : 0.399
dressing bottles [plastic, yellow] : 0.403
energy drink [aluminum can, blue] : 0.381
energy drink [aluminum can, pink] : 0.411
energy drink [aluminum can, yellow] : 0.424
energy drink [aluminum can, orange] : 0.428
chipstar [cardboard, green] : 0.470
chipstar [cardboard, red] : 0.470
chipstar [cardboard, orange] : 0.499
chipstar [cardboard, white] : 0.600
labeled plastic bottle [plastic, white] : 0.479
labeled plastic bottle [plastic, pink] : 0.547
labeled plastic bottle [plastic, orange] : 0.595
labeled plastic bottle [plastic, yellow] : 0.616
Qoo jelly [plastic, purple] : 0.546
Qoo jelly [plastic, pink] : 0.584
Qoo jelly [plastic, green] : 0.588
Qoo jelly [plastic, red] : 0.595
pringles tube [cardboard, green] : 0.662
pringles tube [cardboard, blue] : 0.687
pringles tube [cardboard, red] : 0.691
pringles tube [cardboard, yellow] : 0.696
yogurt [plastic, green] : 0.738
yogurt [plastic, red] : 0.743
yogurt [plastic, pink] : 0.754
yogurt [plastic, purple] : 0.763
tissue box [cardboard, blue] : 0.830
tissue box [cardboard, orange] : 0.860
tissue box [cardboard, green] : 0.909
tissue box [cardboard, pink] : 0.960
paper box [cardboard, orange] : 0.792
paper box [cardboard, yellow] : 0.903
paper box [cardboard, blue] : 0.964
paper box [cardboard, red] : 0.965
pocky [cardborad, green] : 0.495
pocky [cardborad, blue] : 0.625
pocky [cardborad, red] : 0.850
pocky [cardborad, yellow] : 0.944

* Please omit useless information and output only the results.

---

User: get_reflectance(["enagy drink", "yogurt"])
You:
|item|result|
|:--|:--|
|Name|enagy drink|
|Reason|Aluminum is a metal, but its surface is printed with resin ink, which reflects light less than cardboard or mirrors. Since the reflectance of a Pringles tube (Aru Gami) is 0.684, the reflectance of an energy drink (aluminum can) is estimated to be 0.422.|
|**Response**|range: 0.41 - 0.46, prediction: **0.422**|

|item|result|
|:--|:--|
|Name|yogort|
|Reason|Yogurt is a food product, but this is usually packaged in a plastic container. The yogurt container is considered less reflective than a similarly labeled plastic bottle. So, the reflectance of yogurt is greater than 0.616. On the other hand, it is white in color and will reflect relatively well. Therefore, we can expect it to be 0.75.|
|**Response**|range: 0.68 - 0.79, prediction: **0.762**|


---

for word in ["whiteboard eraser", "bristle of a brush", "smartphone", "adhesive", "insect repellent", "zone (can drick)", "candy crackers"]:
    User: get_reflectance(word)
    You:

```

<br><br>

# The output of GPT-4

|item|result|
|:--|:--|
|Name|whiteboard eraser|
|Reason|A whiteboard eraser typically has a plastic body and a felt pad. The plastic could have a similar reflectance to that of labeled plastic bottles, but the felt pad, being a soft and non-glossy material, would reflect less. Given the reflectance values for labeled plastic bottles range from 0.479 to 0.616, and considering the felt's lower reflectance, a reasonable estimate for a whiteboard eraser could be slightly lower.|
|**Response**|range: 0.45 - 0.55, prediction: **0.48**|

|item|result|
|:--|:--|
|Name|bristle of a brush|
|Reason|The bristles of a brush, depending on their material (synthetic or natural), will have a reflectance somewhat akin to that of fabric or less reflective plastics. Given that they are not designed to be reflective and have a matte surface, their reflectance would be lower than that of glossy or semi-transparent plastics.|
|**Response**|range: 0.3 - 0.4, prediction: **0.35**|

|item|result|
|:--|:--|
|Name|smartphone|
|Reason|The reflectance of a smartphone would vary significantly based on its surface material. A smartphone with a glass front and a metallic back could have higher reflectance due to the glass and metal surfaces. Considering the reflectance of aluminum cans as a reference, and knowing that glass can reflect similarly or more depending on the treatment, an average smartphone could have a moderately high reflectance.|
|**Response**|range: 0.4 - 0.5, prediction: **0.45**|

|item|result|
|:--|:--|
|Name|adhesive|
|Reason|Adhesive materials, depending on their type (liquid, gel, or tape), have varying surface characteristics. Most adhesives are designed to be applied thinly and hence would not significantly contribute to the reflectance. However, if we consider the packaging, it would typically be similar to labeled plastic bottles but with less surface area being reflective.|
|**Response**|range: 0.4 - 0.5, prediction: **0.42**|

|item|result|
|:--|:--|
|Name|insect repellent|
|Reason|Insect repellent containers are often made of plastic and might have labels covering much of their surface. Their reflectance could be similar to labeled plastic bottles, but potentially lower due to textural differences and less surface area being smooth and reflective.|
|**Response**|range: 0.45 - 0.55, prediction: **0.5**|

|item|result|
|:--|:--|
|Name|zone (can drink)|
|Reason|Assuming "zone" is a typo for a canned drink, similar to an "energy drink," its aluminum can with printed resin ink would have reflectance values close to those of energy drinks. The reflectance would slightly vary based on the color and design but generally remain within the same range.|
|**Response**|range: 0.41 - 0.46, prediction: **0.43**|

|item|result|
|:--|:--|
|Name|candy crackers|
|Reason|Candy crackers packaging, if made of cardboard like Chipstar or Pocky, would have a reflectance dependent on the color and surface finish. Given that cardboard packaging for snacks tends to have a high reflectance, especially if brightly colored or white, we can expect it to be similar to other high-reflectance cardboard packaging.|
|**Response**|range: 0.49 - 0.96, prediction: **0.72**|
