# IPSOS-assignment

Attached two .csv files – train.csv and test.csv – each containing several columns of input
variables and a single outcome variable, named V1 through VX and “OUTCOME”. Each row in the file
represents an observation for a different individual. You will need to create a function to produce a
predicted value of the outcome variable for each individual in test.csv. For each individual in test.csv,
this prediction will be calculated based on the individuals in train.csv who have similar input values as
that individual (details below). Please write the code in your choice of either R or Python, and feel free
to use any libraries you choose. Your submission should be either a .R file or a .py file. Please convert
notebooks to these formats. Your file should contain only the functions needed to run the algorithm.
Please do not submit a script with data munging or file imports. Assume a user of your code will already
have cleaned and read in the data and only needs to be able to call your API. The call signature of the
main entry-point function to your code should follow this exact form (again, choose either R or Python):

1. Function inputs
· train: A data frame of training cases
· test: A data frame of test cases
· k: The number of similar individuals (“neighbors”) to use in the similarity calculation
· metric: A string specifying the distance metric to use in the similarity calculation (can take on
the values “euclidean” or “manhattan”)
· outcome: A string specifying the name of the outcome variable
In R, the function should look like this:
predict <- function(train, test, k = 5, metric = “euclidean”, outcome = ”OUTCOME”) {…}
While in Python, it should look like this:
def predict(train, test, k = 5, metric = “euclidean”, outcome = "OUTCOME"):

3. Function logic
· For each individual in the test data, you will need to compute their distance to every other
individual in the training data
o Distance should be calculated based on the input variables V1 through VX only (do not
use the outcome variable in the calculation).
o There may or may not be missing input values present in the data we will use to evaluate
your submission. You can assume missing values will always be represented by either
NA or NaN. The distance between two observations should be calculated based on the
non-missing overlap between them. Concretely, if two observations are (1, 2, NA, 4, 5)
and (3, 4, 5, NA, 6) you will ignore both the 3rd and 4th values in BOTH observations for
your calculation of the distance between them, since the 3rd value is missing from the
first observation and the 4th value is missing from the second.
o Do not impute or otherwise fill in the missing data. If there is no non-missing overlap
between any two observations, the distance between them should be NA. If it’s
important to your implementation, you may assume the data is missing-at-random.
o The distance metrics to include in the function are Euclidean / 2-norm and Manhattan /
City Block / 1-norm distance as typically defined, except for the missingness handling as
defined above.
· For each individual in the test data, you will need to select the k most similar individuals
(“neighbors”) to them in the training data, based on the those with the smallest distances as
calculated above.
o If there are ties in the kth place, include all tied individuals as neighbors in addition to any
that came before the kth place, even if this ends up being more than k.
o If the distance to any training individuals is zero, only consider those zero-distance
individuals as neighbors, even if this ends up being more or less than k.
o Further, if the distance to any training individuals is zero, the predicted outcome should
be calculated as the arithmetic mean of the outcome of only those zero-distance
neighbors.
· For each individual in the test data, compute their predicted outcome value as the weighted
average of the outcome variable among the neighbors you have selected for that individual.
o The weight to use in the weighted average is simply the inverse of the distance between
the individual and her/his neighbor.
o For example, if five neighbors have distances of (1, 2, 3, 4, 5) and their outcome values
are (1, 1, 1, 2, 2), respectively, then the predicted outcome would be:
(1, 1/2 , 1/3, 1/4, 1/5) ⋅ (1, 1, 1, 2, 2) / Σ (1, 1/2, 1/3, 1/4, 1/5),
where ⋅ is the vector dot product.
4. Function flexibility
Aside from being able to take the function arguments above, the function or functions must be flexible
enough to handle:
· Any location of the outcome variable (could be the first column, last column, or somewhere in
between)
· Any arbitrary number of input columns (V1-V5, V1-V100, etc.)
· More extreme missingness than seen in the attached data
5. Function outputs
The output of the function should be a vector of predicted outcome values – one value for each
individual in test.csv.
6. Evaluation
Your implementation will be tested against data with a variety of different sizes and missingness
patterns. You will be evaluated on:
· The robustness of your solution to pathological examples
· The performance of your solution on larger datasets
· The quality of your code, specifically:
o Is it easy to read and understand?
o Is it modular (i.e., separated into reusable functions)?
o Is it flexible (i.e., is it sufficiently parametrized that one could make changes easily)?
o Does it conform to best practices in your chosen programming language?
o Is it clean?
7. Some helpful information (test case results)
If you have created the function correctly (with train.csv and test.csv loaded into variables called train
and test, respectively), you should get the following results:
R:
┌────
│ sqrt(mean((test$OUTCOME - predict(train, test, k=5)) ^ 2)) == 0.3638982
└────
Python:
┌────
│ import math
│ math.sqrt(((test["OUTCOME"] - predict(train,test, k=5)) ** 2).mean()) == 0.3638982
└────
If you feel the instructions are ambiguous on any point, please do not reply to this email with
questions (you will not get a response). Instead, please make an assumption based on your best
understanding of the instructions, note the assumption in your code, and move on.
Good luck! We expect this to take between 2 and 3 hours of your time and we thank you for investing
that time in us! Please send a reply, attaching the R or Python code you wrote to execute this
task, within 3 hours of receiving this email. If you finish quickly and are inclined to do more, we would
recommend you polish and comment your code as though this were the core of a production project,
rather than using that extra time to extend the example or implement new features.
Finally, please do not post articles, details, or code about this test publicly.
