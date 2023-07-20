# Optimization of Food Composition for Athletes using Genetic Algorithm

## Overview
Athletes' nutrition is one of the important things for their performance. We create a recommender system used to compose athletes' food ingredients.
We use genetic algorithm technique to optimize the composition by considering
the athlete's needs and the ingredients' price. The output of this program is 15 food ingredients that we hope can be used to make 3 meals a day, with 5 ingredients for each meal. Note that this calculation is only an estimation, expert advice is needed to give the best recommendation.

## Dataset
There are 2 datasets that we use to build the system, namely nutritional data for each food ingredient [1] and food price data. For food price data, we collect data from one of the Indonesian e-commerce in Yogyakarta from October - November 2022. If the ingredients are not available in Yogyakarta, we randomly choose the price. We only use a total of 200 ingredients in this study.
## Algorithm
### Calculate Athlete's needs
Suppose the athlete is a 23 years old male, with  a body weight of 70 kilograms and a height of 175 centimeters. Someday, he will have a light activity.
1. Basal Metabolic Rate (BMR) <br>
    BMR is calories burned while resting. Based on [2], BMR for a male can be calculated by equation (1). <br>
    $`BMR := 66,5 + (13,7 \cdot \text{body weight}) + (5 \cdot\text{body height}) - (6.8 \cdot \text{age}) \text{ (1)}`$ <br><br>
    Thus, his BMR is <br>
    $`BMR = 66,5 + (13,7\cdot70) + (5 \cdot 175) - (6.8 \cdot 23) = 1774.1 \text{ calories}`$
2. Total Energy Expenditure (TEE) <br>
    TEE is the total calories burned while doing activities. Based on [2], TEE can be calculated by equation (2). <br>
    $`TEE := BMR \cdot \text{activity factor} \text{ (2)}`$ <br><br>
    Activity factors can be viewed in the following table.
    |Type of activity | Activity factor|
    |---|---|
    |complete rest|1|
    |very light |1.3|
    |light| male: 1.6 <br> female: 1.5|
    |moderate| male: 1.7 <br> female: 1.6|
    |strenuous| male: 2.1 <br> female: 1.9|
    |very strenuous| male: 2.4 <br> female: 2.2|

    Since his BMR is 1774.1 and he will have light activity, his TEE is <br>
    $`TEE = 1774.1 \cdot 1.6 = 2838.56 \text{ calories}`$
3. Calories needed <br>
   There are three kinds of nutrients that provide calories to the human body, namely carbohydrates, protein, and fat [3]. The percentage of calories provided by each nutrient can be calculated by equations (4), (5), and (6) [4]. <br>
    $`Carbohydrates := 60\% \cdot TEE \text{ (4)} `$ <br>
    $`Protein := 25\% \cdot TEE \text{ (5)} `$ <br>
    $`Fat := 15\% \cdot TEE \text{ (6)}`$ <br><br>
   Since his TEE is 2838.56, then the calories needed by each nutrient is  <br>
    $`Carbohydrates = 60\% \cdot 2838.56 = 1703.136 \text{ calories}`$   <br>
    $`Protein = 25\% \cdot 2838.56 = 709.64 \text{ calories}`$  <br>
    $`Fat = 15\% \cdot 2838.56 = 425.784 \text{ calories}`$  <br><br>
    Because we recommend the ingredients, we only take 95% of the calories needed. We spare the rest 5% for seasoning and some other ingredients that may be needed to make the meal. Thus, the calories needed is <br>
    $`Carbohydrates = 95\% \cdot 1703.136 = 1617.9792 \text{ calories}`$ <br>
    $`Protein = 95\% \cdot 709.64 = 674.158 \text{ calories}`$ <br>
    $`Fat = 95\% \cdot 425.784 = 404.4948 \text{ calories}`$
### Genetic Algorithm
1. Encoding <br>
    We use an integer encoding scheme, where each individual consists of one chromosome. Each chromosome consists of 15 genes representing the indexes of the ingredients. <br>
    $`Chromosome = [gen_1, gen_2, ..., gen_{15}]`$
2. Fitness function <br>
   We formulate fitness functions considering calories needed and ingredients needed. The formula can be seen in equation (7).
   $Fitness = \frac{1}{\frac{\sum prices}{1000} + penalty} \text{ (7)}$ <br>
   $Penalty = \alpha(penalty_{fat}) + \alpha(penalty_{carbs}) + \alpha(penalty_{protein})$ <br>
   $Penalty_{fat, carbs, protein} = |total_{fat, carbs, protein} - needs_{fat, carbs, protein}|$
3. Selection <br>
    We use the tournament selection scheme.
4. Crossover <br>
    We use a multipoint crossover with 2 points that are randomly selected.
5. Mutation <br>
    We use the creep method, where each mutation is done by adding random numbers with values ranging from [-3, 3].
6. Generation update <br>
    We use two schemes of generation updates.
    * Generational model <br>
        In the generational model, the previous generation will be replaced by the new generation.
    * Continuous update <br>
        In the continuous update, the population is selected by randomly choosing the individual from the previous generation and the new generation.
## References
[1] “Data Komposisi Pangan Indonesia - Beranda.”
https://www.panganku.org/id-ID/cari_nutrisi (accessed 20 December 2022).

[2] A. Hartono, Asuhan Nutrisi Rumah Sakit. 1999.

[3] S. Moehji, Ilmu Gizi jilid 2, Cet. 6. Bhratara Karya Aksara, 1982.

[4] M. I. Pratiwi, “Implementasi Algoritma Genetika Pada Optimasi Biaya Pemenuhan
Kebutuhan Gizi,” Bachelor, Universitas Brawijaya, 2014. Accessed: 8 September 2022.
[Online]. Available at: http://repository.ub.ac.id/id/eprint/145956/

## Code reference
[Machine learning mastery](https://machinelearningmastery.com/simple-genetic-algorithm-from-scratch-in-python/)

## Group members
* Nicholas Marojahan S.
* Muhammad Faqih Husaen
* Siti Muslimah K.H.N.
* Zaidurrohman
