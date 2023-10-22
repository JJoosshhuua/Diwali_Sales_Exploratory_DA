# Diwali_Sales_Exploratory_DA

Worked on
 - Changing Data Types
 - Changing Column Names
 - Dropping Zero Columns
 - Replacing Null Values
 - Removing Duplicates
 - Checking Outliers
 - Using IQR Method
 - Removing Outliers
 - Visualising using Bar Plot
 - Finding a Correlation by using a Heatmap
 - Finding out the relationship between variables using a pair plot.

# In[1]:
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# In[2]:
df = pd.read_csv(r'C:\Users\joshua\OneDrive\Desktop\Diwali_Sales.csv', encoding= 'unicode_escape')
# In[3]:
df.describe()
# In[4]:
df.columns
# In[5]:
df.head()
# In[6]:
df.dtypes

# ## CHANGING DTYPES
# In[7]:
df["Product_ID"] = df["Product_ID"].astype(str)
df['Product_ID'].dtypes
# In[8]:
df.dtypes

# ## CHANGING COLUMN NAMES
# In[9]:
df.rename(columns = {"Age Group" : "Age_Group"}, inplace=True)
df.columns
# In[10]:
df.size
# In[11]:
df.shape
# In[12]:
df.isna().sum()
# In[13]:
# Status and unnamed columns are completely blank. They are considered Zero-columns and will be ignored from the study

# ## DROP ZERO COLUMNS
# In[14]:
df.drop(['Status', 'unnamed1'], axis=1, inplace = True)
df.isna().sum()
# In[15]:
df['Amount'].mean()

# ## REPLACING NULL VALUES
# In[16]:
F
df.isna().sum()
# In[17]:
df.loc[df.duplicated()]

# ## REMOVING DUPLICATES
# In[18]:
df = df.drop_duplicates()
df.duplicated().sum()
# In[19]:
df.head()

# ## CHECK FOR OUTLIERS
# In[20]:
plt.boxplot(df['Age'])
# In[21]:
q1 = df['Age'].quantile(0.25)
q3 = df['Age'].quantile(0.75)
q1,q3
# In[22]:
iqr = q3-q1
iqr
# In[23]:
UB = q3+iqr*1.5
LB = q1-iqr*1.5
UB, LB
# In[24]:
df[df['Age']>UB]
# In[25]:
df[df['Age']<LB]
# In[26]:
df[(df['Age']>UB)|(df['Age']<LB)]
# In[27]:
No_outliers = df[~((df['Age']>UB )| (df['Age']<LB))]
No_outliers
# In[28]:
No_outliers.shape
# In[29]:
df.shape
# In[30]:
sns.kdeplot(df['Age'])
# In[31]:
sns.kdeplot(No_outliers['Age'])
# In[32]:
plt.boxplot(No_outliers['Orders'])
# In[33]:
plt.boxplot(No_outliers['Amount'])
# In[34]:
Q1 = No_outliers['Amount'].quantile(0.25)
Q3 = No_outliers['Amount'].quantile(0.75)
Q1,Q3
# In[35]:
IQR = Q3-Q1
IQR
# In[36]:
Upper = Q3+IQR*1.5
Lower = Q1-IQR*1.5
Upper, Lower
# In[37]:
No_outliers[No_outliers['Amount']>Upper]
# In[38]:
No_outliers[No_outliers['Amount']<Lower]
# In[40]:
NO = No_outliers[~((No_outliers['Amount']>Upper) | (No_outliers['Amount']<Lower))]
NO
# In[41]:
sns.kdeplot(NO['Amount'])
# In[42]:
plt.boxplot(NO['Amount'])
# In[44]:
NO[NO['Amount']>Upper]

# ## EXPLORATORY DATA ANALYSIS
# In[45]:
a = sns.countplot(x = 'Gender', data = NO)
# In[47]:
a = sns.countplot(x = 'Gender', data = NO)
for bars in a.containers:
a.bar_label(bars)
# In[48]:
NO.head(2)
# In[ ]:
# In[89]:
b = sns.countplot(x = 'State', data = NO)
sns.set(rc = {'figure.figsize': (15,5)})
for bars in b.containers:
b.bar_label(bars)
# In[72]:
c = sns.countplot(x = 'Zone', data = NO, hue='Marital_Status')
for bars in c.containers:
c.bar_label(bars)
# In[69]:
d = sns.countplot(x = 'Age_Group', data = NO, hue = 'Gender')
for bars in d.containers:
d.bar_label(bars)
# In[68]:
e = sns.countplot(x = 'Marital_Status', data = NO, hue='Gender')
for bars in e.containers:
e.bar_label(bars)
# In[88]:
f = sns.countplot(x = 'Product_Category', data = NO)
sns.set(rc = {'figure.figsize': (30,10)})
for bars in f.containers:
f.bar_label(bars)
# In[101]:
g = sns.countplot(x='Age', data = NO)
sns.set(rc = {'figure.figsize': (15,5)})
for bars in g.containers:
g.bar_label(bars)
# In[58]:
sns.pairplot(NO)
# In[90]:
CR = NO.corr()
sns.set(rc = {'figure.figsize': (15,5)})
sns.heatmap(CR, xticklabels = NO.columns, yticklabels = NO.columns, vmin = -1, vmax= 1, annot = True)
plt.show
# In[92]:
sns.set(rc = {'figure.figsize': (15,5)})
NO.groupby(['Gender'], as_index = False)['Amount'].sum().sort_values(by = 'Amount', ascending = False)
# In[94]:
X = NO.groupby(['Gender'], as_index = False)['Amount'].sum().sort_values(by = 'Amount', ascending = False)
sns.set(rc = {'figure.figsize': (15,5)})
Y = sns.barplot(x='Gender', y = 'Amount', data=X)
for bars in Y.containers:
Y.bar_label(bars)
# In[95]:
P = NO.groupby(['Age_Group'], as_index = False)['Amount'].sum().sort_values(by = 'Amount', ascending = False)
sns.set(rc = {'figure.figsize': (15,5)})
Q = sns.barplot(x='Age_Group', y = 'Amount', data=P)
for bars in Q.containers:
Q.bar_label(bars)
# In[78]:
sales_State = NO.groupby(['State'], as_index = False)['Amount'].sum().sort_values(by = 'Amount', ascending = False)
sns.set(rc = {'figure.figsize':(20,5)})
Z = sns.barplot(x = 'State', y = 'Amount', data = sales_State)
for bars in Z.containers:
Z.bar_label(bars)
# In[99]:
Sales_State_Orders = NO.groupby(['State'], as_index = False)['Orders'].sum().sort_values(by = 'Orders', ascending = False)
sns.set(rc = {'figure.figsize' : (25,5)})
Q = sns.barplot(x='State', y='Orders', data = Sales_State_Orders)
for bars in Q.containers:
Q.bar_label(bars)

# ## CONCLUSION
#Women have more purchases than Men and spent higher than men
#UP and Maharastra have more purchases, and more orders than rest of the states and spent the most comparatively.
#Central Zonw has more purchases.
#Singles are making a lot of purchases compared to the married
#26-35 group of people are making a lot of purchases and spent higher than other groups.
#Clothing&Apparel, Food and Electronics are top 3 categories that are sold highest
#There is no strong correlation found among any 2 variables
