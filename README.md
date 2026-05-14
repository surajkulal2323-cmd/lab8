## Program 1

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import fetch_california_housing

housing_data = fetch_california_housing(as_frame=True)
data = housing_data.frame

print(data.head())

numerical_features = data.select_dtypes(include=['float64', 'int64']).columns

print("Numerical Features:")
print(numerical_features)

for feature in numerical_features:
    plt.figure(figsize=(8,5))
    plt.hist(data[feature], bins=30, color='skyblue', edgecolor='black')
    plt.title(f'Histogram of {feature}')
    plt.xlabel(feature)
    plt.ylabel('Frequency')
    plt.grid(True)
    plt.show()

for feature in numerical_features:
    plt.figure(figsize=(8,5))
    sns.boxplot(x=data[feature], color='lightgreen')
    plt.title(f'Boxplot of {feature}')
    plt.xlabel(feature)
    plt.show()

    Q1 = data[feature].quantile(0.25)
    Q3 = data[feature].quantile(0.75)

    IQR = Q3 - Q1

    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    outliers = data[(data[feature] < lower_bound) | (data[feature] > upper_bound)]

    print(f'Outliers in {feature}')
    print(outliers[feature])
```

---

## Program 2

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing

housing_data = fetch_california_housing(as_frame=True)

data = housing_data.frame

print(data.head())

correlation_matrix = data.corr()

print("Correlation Matrix")
print(correlation_matrix)

plt.figure(figsize=(10,8))

sns.heatmap(
    correlation_matrix,
    annot=True,
    cmap='coolwarm',
    fmt='.2f',
    linewidths=0.5
)

plt.title("Correlation Matrix Heatmap")

plt.show()

sns.pairplot(
    data,
    diag_kind='kde',
    plot_kws={'alpha':0.7}
)

plt.show()
```

---

## Program 3

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.decomposition import PCA

iris = load_iris()

df = pd.DataFrame(
    data=iris.data,
    columns=iris.feature_names
)

pca = PCA(n_components=2)

principal_components = pca.fit_transform(df)

principal_df = pd.DataFrame(
    data=principal_components,
    columns=['Principal Component 1', 'Principal Component 2']
)

final_df = pd.concat(
    [
        principal_df,
        pd.DataFrame(data=iris.target, columns=['target'])
    ],
    axis=1
)

fig = plt.figure(figsize=(8,6))

ax = fig.add_subplot(1,1,1)

ax.set_xlabel('Principal Component 1', fontsize=15)
ax.set_ylabel('Principal Component 2', fontsize=15)

ax.set_title('2 Component PCA', fontsize=20)

targets = [0,1,2]

colors = ['r','g','b']

for target, color in zip(targets, colors):

    indices = final_df['target'] == target

    ax.scatter(
        final_df.loc[indices, 'Principal Component 1'],
        final_df.loc[indices, 'Principal Component 2'],
        c=color,
        s=50
    )

ax.legend(iris.target_names)

ax.grid()

plt.show()

print("Explained Variance Ratio:")
print(pca.explained_variance_ratio_)
```

---

## Program 4

```python
import csv

attributes = [
    ['Sunny','Rainy'],
    ['Warm','Cold'],
    ['Normal','High'],
    ['Strong','Weak'],
    ['Warm','Cool'],
    ['Same','Change']
]

num_attributes = len(attributes)

print("Most General Hypothesis")
print(['?'] * num_attributes)

print("Most Specific Hypothesis")
print(['0'] * num_attributes)

training_data = []

with open('ws.csv', 'r') as csvFile:

    reader = csv.reader(csvFile)

    for row in reader:
        training_data.append(row)
        print(row)

hypothesis = ['0'] * num_attributes

print("Initial Hypothesis")
print(hypothesis)

for i in range(num_attributes):
    hypothesis[i] = training_data[0][i]

print("Initial Positive Hypothesis")
print(hypothesis)

for i in range(len(training_data)):

    if training_data[i][num_attributes] == 'Yes':

        for j in range(num_attributes):

            if training_data[i][j] != hypothesis[j]:
                hypothesis[j] = '?'

            else:
                hypothesis[j] = training_data[i][j]

    print("Hypothesis for Training Example", i + 1)
    print(hypothesis)

print("Final Hypothesis")
print(hypothesis)
```

---

## ws.csv

```csv
Sunny,Warm,Normal,Strong,Warm,Same,Yes
Sunny,Warm,High,Strong,Warm,Same,Yes
Rainy,Cold,High,Strong,Warm,Change,No
Sunny,Warm,High,Strong,Cool,Change,Yes
```

---

## Program 5

```python
import numpy as np
from sklearn.neighbors import KNeighborsClassifier

data = np.random.rand(100)

labels = np.zeros(100)

labels[:50] = np.where(data[:50] <= 0.5, 1, 2)

train_data = data[:50].reshape(-1,1)

train_labels = labels[:50]

test_data = data[50:].reshape(-1,1)

k_values = [1,2,3,4,5,20,30]

for k in k_values:

    knn = KNeighborsClassifier(n_neighbors=k)

    knn.fit(train_data, train_labels)

    predicted_labels = knn.predict(test_data)

    print("K =", k)

    print("Test Value\tPredicted Label")

    for value, label in zip(test_data.flatten(), predicted_labels):

        print(f"{value:.3f}\t\t{int(label)}")

    print()
```

---

## Program 8

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

data = load_breast_cancer()

X = data.data

y = data.target

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

classifier = DecisionTreeClassifier(random_state=42)

classifier.fit(X_train, y_train)

y_pred = classifier.predict(X_test)

accuracy = accuracy_score(y_test, y_pred)

print("Accuracy")
print(accuracy)

new_sample = np.array([
    [
        17.99,10.38,122.8,1001.0,0.1184,
        0.2776,0.3001,0.1471,0.2419,0.07871,
        1.095,0.9053,8.589,153.4,0.006399,
        0.04904,0.05373,0.01587,0.03003,0.006193,
        25.38,17.33,184.6,2019.0,0.1622,
        0.6656,0.7119,0.2654,0.4601,0.1189
    ]
])

prediction = classifier.predict(new_sample)

print("Prediction")
print(data.target_names[prediction][0])
```

---

## Program 9

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_olivetti_faces
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score

faces = fetch_olivetti_faces()

X = faces.images

y = faces.target

X = X.reshape(X.shape[0], -1)

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

classifier = GaussianNB()

classifier.fit(X_train, y_train)

y_pred = classifier.predict(X_test)

accuracy = accuracy_score(y_test, y_pred)

print("Accuracy")
print(accuracy)

plt.figure(figsize=(12,4))

for i in range(5):

    plt.subplot(1,5,i+1)

    plt.imshow(
        X_test[i].reshape(64,64),
        cmap='gray'
    )

    plt.title(f"T:{y_test[i]}\nP:{y_pred[i]}")

    plt.axis('off')

plt.show()
```

---

## Program 10

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA

data = load_breast_cancer()

X = data.data

y = data.target

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)

kmeans = KMeans(
    n_clusters=2,
    random_state=42,
    n_init=10
)

kmeans.fit(X_scaled)

labels = kmeans.labels_

pca = PCA(n_components=2)

X_pca = pca.fit_transform(X_scaled)

plt.figure(figsize=(8,6))

plt.scatter(
    X_pca[:,0],
    X_pca[:,1],
    c=labels,
    cmap='viridis',
    alpha=0.7
)

plt.xlabel('Principal Component 1')

plt.ylabel('Principal Component 2')

plt.title('K-Means Clustering')

plt.colorbar(label='Cluster')

plt.show()
```
