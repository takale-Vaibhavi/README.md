1) Write a program to solve a classic ancient Chinese puzzle: We count 35 heads and 94 legs among the chickens and rabbits in a farm. How many rabbits and how many chickens do we have?

for r in range(36):
    c = 35 - r
    if 4*r + 2*c == 94:
        print("Rabbits:", r)
        print("Chickens:", c)

Q.2) You are given an integer, N. Your task is to print an alphabet rangoli of size N. (Rangoli is a form of Indian folk art based on creation of patterns.) Different sizes of alphabet rangoli are shown below:
import string

n = int(input("Enter a number: "))

alpha = string.ascii_lowercase

for i in range(n):
    s = "-".join(alpha[i:n])
    print((s[::-1] + s[1:]).center(4*n-3, "-"))

for i in range(n-2, -1, -1):
    s = "-".join(alpha[i:n])
    print((s[::-1] + s[1:]).center(4*n-3, "-"))

Q.3) Please write a program to print the running time of execution of "1+1" for 100 times.

import time

start = time.time()

for i in range(100):
    x = 1 + 1

end = time.time()

print("Time:", end - start)

Q.4) Using python Normalize a 5x5 random matrix

import numpy as np

A = np.random.random((5,5))
A = (A - A.min())/(A.max() - A.min())

print(A)

Q.5) Extract the integer part of a random array of positive numbers using 4 different methods.

import numpy as np

A = np.random.random(5)*10

print(np.floor(A))
print(A.astype(int))
print(np.trunc(A))
print([int(i) for i in A])

Q.6) Consider a generator function that generates 10 integers and use it to build an array

import numpy as np

def gen():
    for i in range(10):
        yield i

A = np.array(list(gen()))
print(A)

Q.7) Consider two random array A and B, check if they are equal 

import numpy as np

A = np.random.randint(0,5,5)
B = np.random.randint(0,5,5)

print(np.array_equal(A,B))

Q.8) Consider a random 10x2 matrix representing cartesian coordinates, convert them 
to polar coordinates. 

import numpy as np

A = np.random.random((10,2))

x = A[:,0]
y = A[:,1]

r = np.sqrt(x**2 + y**2)
theta = np.arctan2(y,x)

print(r,theta)

Q.9) Generate a dataset of 100 random numbers representing exam scores. 
Using Matplotlib: 
● Plot a histogram of the scores. 
● Show the distribution of marks. 
● Label axes and title. 

import numpy as np
import matplotlib.pyplot as plt

scores = np.random.randint(0,100,100)

plt.hist(scores)
plt.xlabel("Marks")
plt.ylabel("Frequency")
plt.title("Distribution of Scores")
plt.show()

Q.10) Create a Python program that generates four different plots in a single figure 
using subplots: 
1. Line Plot 
2. Bar Chart 
3. Histogram 
4. Scatter Plot 
Use randomly generated data. 

import matplotlib.pyplot as plt
import numpy as np

x = np.arange(5)
y = np.random.randint(1,10,5)

plt.figure()

plt.subplot(2,2,1)
plt.plot(x,y)

plt.subplot(2,2,2)
plt.bar(x,y)

plt.subplot(2,2,3)
plt.hist(np.random.randn(100))

plt.subplot(2,2,4)
plt.scatter(x,y)

plt.show()

Q.11) Create a visualization dashboard using Matplotlib that includes: 
● Sales Line Chart 
● Profit Bar Chart 
● Product Distribution Pie Chart 
All plots should appear in one figure using subplots. 

import matplotlib.pyplot as plt

sales = [10,20,30,40]
profit = [5,10,15,20]
products = [40,30,20,10]

plt.figure()

plt.subplot(2,2,1)
plt.plot(sales)

plt.subplot(2,2,2)
plt.bar(range(4),profit)

plt.subplot(2,2,3)
plt.pie(products)

plt.show()

Q.12) Generate a dataset of 200 random exam scores between 0 and 100. 
1. Create a Pandas DataFrame for the scores. 
2. Plot a histogram using Seaborn. 
3. Interpret the distribution of scores. 

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

scores = np.random.randint(0,100,200)

df = pd.DataFrame(scores, columns=["Marks"])

sns.histplot(df["Marks"])
plt.show()

Q.13) Generate a dataset of student marks from three different classes. 
1. Store the data using Pandas. 
2. Use Seaborn boxplot to compare the distribution of marks among the classes. 

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

data = {
    "ClassA": np.random.randint(0,100,30),
    "ClassB": np.random.randint(0,100,30),
    "ClassC": np.random.randint(0,100,30)
}

df = pd.DataFrame(data)

sns.boxplot(data=df)
plt.show()

Q.14) Given a dataset of 100 students with multiple attributes. 
1. Use Pandas to display summary statistics. 
2. Plot: 
o Histogram of marks 
o Boxplot for outliers 
o Pairplot using Seaborn
3. Identify patterns in the dataset. 

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

data = pd.DataFrame({
    "Marks": np.random.randint(0,100,100),
    "Study": np.random.randint(1,10,100),
    "Sleep": np.random.randint(4,10,100)
})

print(data.describe())

sns.histplot(data["Marks"])
plt.show()

sns.boxplot(data["Marks"])
plt.show()

sns.pairplot(data)
plt.show()

Q.15) Generate a dataset of 200 random values representing exam scores. 
1. Calculate mean and standard deviation. 
2. Plot a histogram using Seaborn. 
3. Interpret whether the distribution is normal or skewed.

import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

scores = np.random.randint(0,100,200)

print("Mean:", np.mean(scores))
print("Std Dev:", np.std(scores))

sns.histplot(scores)
plt.show()

