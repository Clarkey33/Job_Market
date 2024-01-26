
import pandas as pd
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns


# # Linkedin Canada: Data Science Jobs 2024

# ## Applying the CRISP-DM Life Cycle
# ###  1. Business Understaning
# ###  2. Data Understanding
# ###  2. Data Preparation 
# ###  3. Modelling
# ###  4. Evaluation
# ###  5. Deployment

# ## Business Understanding
# 
# #### The dataset is quite niche and focuses on the job market specific to the Data Science Field. The records were collated from Linkedin which is but one out of several job boards; there should be much caution in how the results of this analysis is extrapolated to a larger grouping.
# 
# #### The Busniess Problem: Is there sufficient supply of candidates for the available Data Science Job?
# ####                       Job offerings are marketed to exclude what is available in the market
# 

# # Data Preparation

# In[43]:


# load data as dataframe


# In[44]:


job_data= pd.read_csv('linkedin_canada.csv')


# In[45]:


#preview dataframe (df)

job_data.head(3)


# In[46]:


#check size of df (records x columns (features x attributes)
job_data.size


# In[47]:


#shape of df
job_data.shape


# In[48]:


#General info on df
job_data.info()


# In[49]:


#check for empty values

job_data.isnull().sum()


# In[50]:


job_data.tail(5
             )


# In[51]:


#check for unique values
job_data.nunique()


# In[52]:


#Check salary column for unique values
job_data.salary.unique()


# #### Based on the presentation of the data in salary, one thought was to seperate the column into a lower salary and upper salary range. However considering there are only 4% of records in this attribute (not statistically significant), it may prove prudent to drop the column.

# In[53]:


#Check applicationsCount column for unique values
job_data.applicationsCount.unique()


# In[62]:


#check number of values 
job_data.applicationsCount.value_counts()


# #### ApplicationsCount column will be converted to a categorical column; the categories are
# #### > 200,<=200, <=175, <=150, <=125, <=100, <=75, <=50, <=25

# In[54]:


#Check contractType column for unique values
job_data.contractType.unique()


# #### contractType will be converted to categorical; only a small number of different values; should result in less memory used
# #### for the empty value will change to "not diclosed"

# In[55]:


#Check experienceLevel column for unique values
job_data.experienceLevel.unique()


# #### Similar to contractType, experienceLevel will be converted to categorical

# #### From a visual look at "location", the attribute will not be changed from object/str. However, the data currently captures City,province, Country. It may serve best to disaggregate into city, province and Country. Where there is no province or city provided the following phrase will be used "not stated".

# In[56]:


#Check postedTime column for unique values
job_data.postedTime.unique()


# #### postedTime will convert to categorical; 1 months ago, 2 months ago, 3 months ago, 4 months ago, 5 months ago, 6 months ago, 7 months ago, 9 months ago

# In[57]:


#Check workType column for unique values
job_data.workType.unique()


# #### workType no changes to be made to this column as well as title, sector and description(for now)

# In[58]:


#check data types in df; note this was implicitly done when .info() was called.
print(job_data.dtypes)


# 

# In[59]:


# Drop the column/attribute "salary"
job_data.drop(columns=['salary'], inplace= True)


# In[61]:


#checking that salary was removed.
job_data.columns


# In[67]:


#Remove all letters from applicationsCount; this will allow us to conver to int and then bin
job_data['applicationsCount'] = job_data['applicationsCount'].str.replace('\D',"",regex= True)


# In[71]:


job_data[['applicationsCount']]


# In[72]:


#convert applicationsCount to int data type
job_data['applicationsCount'] = job_data['applicationsCount'].astype(int)


# In[73]:


job_data['applicationsCount'].dtypes


# In[84]:


#Binning numerical variables for applicationsCount
job_data.loc[job_data['applicationsCount'].between(0, 25, 'both'), 'Num_Applicants'] = '<=25'
job_data.loc[job_data['applicationsCount'].between(25, 50, 'right'), 'Num_Applicants'] = '<=50'
job_data.loc[job_data['applicationsCount'].between(50, 75, 'right'), 'Num_Applicants'] = '<=75'
job_data.loc[job_data['applicationsCount'].between(75, 100, 'right'), 'Num_Applicants'] = '<=100'
job_data.loc[job_data['applicationsCount'].between(100, 125, 'right'), 'Num_Applicants'] = '<=125'
job_data.loc[job_data['applicationsCount'].between(125, 150, 'right'), 'Num_Applicants'] = '<=150'
job_data.loc[job_data['applicationsCount'].between(150, 175, 'right'), 'Num_Applicants'] = '<=175'
job_data.loc[job_data['applicationsCount'].between(175, 200, 'left'), 'Num_Applicants'] = '<=200'
job_data.loc[job_data['applicationsCount'].between(200,500,  'left'), 'Num_Applicants'] = '>200'


# In[85]:


job_data[['applicationsCount', 'Num_Applicants']]


# In[ ]:
