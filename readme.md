# Optimization of Food Composition for Athletes using Genetic Algorithm

## Overview
Athlete's nutrition is one of the important things for their performance. We create a recommender system used to compose athlete's food ingredients.
We use genetic algorithm technique to optimize the composition by considering
the athlete's needs and the ingredients' price. The output of this program is 15 food ingredients that we hope can be used to make 3 meals a day, with 5 ingredients for each meal. Note that this calculation is only an estimation, expert advice is needed to give the best recommendation.

## Dataset
There are 2 datasets that we use to build the system, namely nutritional data for each food ingredient [1] and food price data. For food price data, we collect the data from one of Indonesian e-commerce in Yogyakarta from October - November 2022. If the ingredients not available in Yogyakarta, we randomly choose the price. We only use a total of 200 ingredients in this study.
## Algorithm
### Calculate Athlete's needs
Suppose the athlete is a 23 years old male, with  a body weight of 70 kilograms and a height of 175 centimeters. Someday, he will have a light activity.
1. Basal Metabolic Rate (BMR) <br>
    BMR is a calories burned while resting. Based on [2], BMR for a male can be calculated by equation (1).
    $$
    BMR := 66,5 + (13,7 \cdot \text{body weight}) + (5 \cdot\text{body height}) - (6.8 \cdot \text{age}) \text{ (1)}
    $$
    Thus, his BMR is
    $$
    BMR = 66,5 + (13,7\cdot70) + (5 \cdot 175) - (6.8 \cdot 23)
    = 1774.1 \text{ calories}
    $$
2. Total Energy Expenditure (TEE) <br>
    TEE is total calories burned while doing activites. Based on [2], TEE for can be calculated by equation (2).
    $$
    TEE := BMR \cdot \text{activity factor} \text{ (2)}
    $$
    Activity factor can be viewed in the following table.
    |Type of activity | Activity factor|
    |---|---|
    |complete rest|1|
    |very light |1.3|
    |light| male: 1.6 <br> female: 1.5|
    |moderate| male: 1.7 <br> female: 1.6|
    |strenuous| male: 2.1 <br> female: 1.9|
    |very strenuous| male: 2.4 <br> female: 2.2|

    Since his BMR is 1774.1 and he will have a light activity, his TEE is
    $$
    TEE = 1774.1 \cdot 1.6 = 2838.56
    $$
3. Calories needed <br>
   There are three kinds of nutrients that provide calories to the human body, namely carbohydrates, protein, and fat [3]. The percentage of calories provided by each nutrients can be calculated by equation (4), (5), and (6) [4].
    $$
    Carbohydrates := 60\% \cdot TEE \text{ (4)} \\
    Protein := 25\% \cdot TEE \text{ (5)} \\
    Fat := 15\% \cdot TEE \text{ (6)}
   $$
   Since his TEE is 2838.56, then the calories needed by each nutrients is
    $$
    Carbohydrates = 60\% \cdot 2838.56 = 1703.136\\
    Protein = 25\% \cdot 2838.56 = 709.64\\
    Fat = 15\% \cdot 2838.56 = 425.784
   $$
    Because we recommeding the ingredients, we only take 95% of the calories needed. We spare the rest 5% for seasoning and some other ingredients that may be needed to make the meal. Thus, the calories needed is 
    $$
    Carbohydrates = 95\% \cdot 1703.136 = 1617.9792\\
    Protein = 95\% \cdot 709.64 = 674.158\\
    Fat = 95\% \cdot 425.784 = 404.4948
   $$
### Genetic Algorithm
1. Encoding <br>
    We use integer encoding scheme, where each individual consists of one chomosome. Each chromosome consists of 15 genes representing the indexes of the ingredients.
    $$
    Chromosome = [gen_1, gen_2, ..., gen_{15}]
    $$
2. Fitness function <br>
   We formulate fitness function considering calories needed and ingredients needed. The formula can be seen in equation (7).
   $$
   Fitness = \frac{1}{\frac{\sum prices}{1000} + penalty} \text{ (7)} \\
   Penalty = \alpha(penalty_{fat}) + \alpha(penalty_{carbs}) + \alpha(penalty_{protein}) \\
   Penalty_{fat, carbs, protein} = |total_{fat, carbs, protein} - needs_{fat, carbs, protein}|
   $$

3. Selection <br>
    We use tournament selection scheme.
4. Crossover <br>
    We use multipoint crossover with 2 points that randomly selected.
5. Mutation <br>
    We use creep method, where each mutation is done by adding random numbers with value ranging from [-3, 3].
6. Generation update <br>
    We use two scheme of generation update.
    * Generational model <br>
        In generational update, the previous generation will be replaced by the new generation.
    * Continuous update <br>
        In continuous update, the population is selected by randomly choose the individual from previous generation and the new generation.
## References
[1] “Data Komposisi Pangan Indonesia - Beranda.”
https://www.panganku.org/id-ID/cari_nutrisi (accessed 20 Desember 2022).

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