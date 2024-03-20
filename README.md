# U.S. Medical Insurance Costs

In this project, a **CSV** file with medical insurance costs will be investigated using Python fundamentals. The goal with this project will be to analyse various attributes within **insurance.csv** to learn more about the patient information in the file and gain insight into potential use cases for the dataset.


```python
# import csv library

import csv
```

To start, all necessary libraries must be imported. For this project the only library needed is the `csv` library in order to work with the **insurance.csv** data. There are other potential libraries that could help with this project; however, for this analysis, using just the `csv` library will suffice.

Next, we'll examine the **insurance.csv** file to familiarise ourselves with the data. We'll review the following aspects of the data file to strategise how to import it into a Python file:

- Names of columns and rows.
- Any noticeable missing data.
- Types of values (numerical vs. categorical).


```python
#Create empty lists for the various attributes in insurance.csv
ages = []
sexes = []
bmis = []
num_children = []
smoker_statuses = []
regions = []
insurance_charges = []
```

The **insurance.csv** file comprises the following columns:

- Patient Age
- Patient Sex
- Patient BMI
- Patient Number of Children
- Patient Smoking Status
- Patient U.S Geographic Region
- Patient Yearly Medical Insurance Cost

No missing data has been detected. To store this information, we'll create seven empty lists, each to hold the data from individual columns of **insurance.csv**.


```python
# helper function to load csv data
def load_list_data(lst, csv_file, column_name):
    # open csv file
    with open(csv_file) as csv_info:
        # read the data from the csv file
        csv_dict = csv.DictReader(csv_info)
        # loop through the data in each row of the csv 
        for row in csv_dict:
            # add the data from each row to a list
            lst.append(row[column_name])
        # return the list
        return lst
```

The helper function mentioned above was designed to streamline the process of loading data into the lists efficiently. Without this function, one would need to manually open **insurance.csv** and rewrite the `for` loop seven times. However, with this function in place, one can simply call `load_list_data()` each time, as demonstrated below.


```python
# look at the data in insurance_csv_dict
load_list_data(ages, 'insurance.csv', 'age')
load_list_data(sexes, 'insurance.csv', 'sex')
load_list_data(bmis, 'insurance.csv', 'bmi')
load_list_data(num_children, 'insurance.csv', 'children')
load_list_data(smoker_statuses, 'insurance.csv', 'smoker')
load_list_data(regions, 'insurance.csv', 'region')
load_list_data(insurance_charges, 'insurance.csv', 'charges')
```

Now that all the data from **insurance.csv** is neatly organised into labeled lists, the analysis can begin. This is where we plan what to investigate and how to conduct the analysis. There are various aspects of the data that could be explored. The following operations will be implemented:

- Find the average age of the patients.
- Return the number of males vs. females counted in the dataset.
- Determine the geographical location of the patients.
- Calculate the average yearly medical charges of the patients.
- Create a dictionary containing all patient information.

To execute these analyses, a class named `PatientsInfo` has been constructed. This class includes five methods:

- `analyze_ages()`
- `analyze_sexes()`
- `unique_regions()`
- `average_charges()`
- `create_dictionary()`

The class implementation is provided below.


```python
class PatientsInfo:
    # init method that takes in each list parameter
    def __init__(self, patients_ages, patients_sexes, patients_bmis, patients_num_children, 
                 patients_smoker_statuses, patients_regions, patients_charges):
        self.patients_ages = patients_ages
        self.patients_sexes = patients_sexes
        self.patients_bmis = patients_bmis
        self.patients_num_children = patients_num_children
        self.patients_smoker_statuses = patients_smoker_statuses
        self.patients_regions = patients_regions
        self.patients_charges = patients_charges

    # method that calculates the average ages of the patients in insurance.csv
    def analyze_ages(self):
        # initialize total age at zero
        total_age = 0
        # iterate through all ages in the ages list
        for age in self.patients_ages:
            # sum of the total age
            total_age += int(age)
        # return total age divided by the length of the patient list
        return ("Average Patient Age: " + str(round(total_age/len(self.patients_ages), 2)) + " years")

    # method that calculates the number of males and females in insurance.csv
    def analyze_sexes(self):
        # initialize number of males and females to zero
        females = 0
        males = 0
        # iterate through each sex in the sexes list
        for sex in self.patients_sexes:
            # if female add to female variable
            if sex == 'female':
                females += 1
            # if male add to male variable
            elif sex == 'male':
                males += 1
        # print out the number of each
        print("Count for female: ", females)
        print("Count for male: ", males)

    # method to find each unique region patients are from
    def unique_regions(self):
        # initialize empty list
        unique_regions = []
        # iterate through each region in regions list
        for region in self.patients_regions:
            # if the region is not already in the unique regions list
            # then add it to the unique regions list
            if region not in unique_regions: 
                unique_regions.append(region)
        # return unique regions list
        return unique_regions

    # method to find average yearly medical charges for patients in insurance.csv
    def average_charges(self):
        # initialize total_charges variable
        total_charges = 0
        # iterate through charges in patients charges list
        # add each charge to total_charge
        for charge in self.patients_charges:
            total_charges += float(charge)
        # return the average charges rounded to the hundredths place
        return ("Average Yearly Medical Insurance Charges: " +  
                str(round(total_charges/len(self.patients_charges), 2)) + " dollars.")
    
    # method to create dictionary with all patients information
    def create_dictionary(self):
        self.patients_dictionary = {}
        self.patients_dictionary["age"] = [int(age) for age in self.patients_ages]
        self.patients_dictionary["sex"] = self.patients_sexes
        self.patients_dictionary["bmi"] = self.patients_bmis
        self.patients_dictionary["children"] = self.patients_num_children
        self.patients_dictionary["smoker"] = self.patients_smoker_statuses
        self.patients_dictionary["regions"] = self.patients_regions
        self.patients_dictionary["charges"] = self.patients_charges
        return self.patients_dictionary
```

The next step involves creating an instance of the class called `patient_info`. With this instance, each method can be invoked to observe the results of the analysis.


```python
patient_info = PatientsInfo(ages, sexes, bmis, num_children, smoker_statuses, regions, insurance_charges)
```


```python
patient_info.analyze_ages()
```

The average age of the patients in **insurance.csv** is approximately 39 years old. This is a crucial check to ensure that the data in **insurance.csv** is representative of a broader population. If the dataset is to be utilised for making inferences about other populations, it must be abundant and broad enough for such use cases.

Further analysis would be necessary to ascertain whether the range and standard deviation of the patient age group in **insurance.csv** are indicative of a random sampling of individuals. This would provide more confidence in the dataset's suitability for broader inference purposes.


```python
patient_info.analyze_sexes()
```

The next step of the analysis involves checking the balance of males vs. females in **insurance.csv**. Similar to the previous step, it is important to ensure that this dataset is representative of a broader population of individuals. If one were to use this dataset to create a classification model, it would be imperative to ensure that the attributes are balanced.

In many real-world scenarios, data imbalance is a common issue. This imbalance can lead to statistical challenges during analysis.


```python
patient_info.unique_regions()
```

There are four unique geographical regions in this dataset, and it's worth noting that all the patients come from the United States. This information provides context about the geographic distribution of the patients and confirms that the dataset represents a single country, the United States.


```python
patient_info.average_charges()
```

The average yearly medical insurance charge per individual is $13,270. Further analysis could be conducted to determine which patient attributes contribute most strongly to low and/or high medical insurance charges. For instance, one could investigate whether patient age correlates with the amount of money they spend yearly on medical insurance. This exploration could help identify factors that significantly influence insurance charges and potentially inform decisions regarding pricing, risk assessment, and healthcare policy.


```python
patient_info.create_dictionary()
```

Now that all patient data is neatly organised in a dictionary, further analysis becomes more convenient. This format facilitates continued investigations into the attributes present in **insurance.csv**. Having the data structured in a dictionary allows for easy access to individual patient information, making it simpler to conduct additional analyses or extract insights as needed.

## Conclusions:

- Average Patient Age: The average age of patients in the dataset is approximately 39 years old, suggesting a relatively middle-aged demographic. This finding provides valuable insight into the age distribution of individuals covered by medical insurance.

- Gender Distribution: The dataset contains information on the balance between males and females. Ensuring a representative balance of gender attributes is essential, especially for classification models. In this dataset, further exploration may be needed to assess the impact of gender on insurance charges or other variables.

- Geographical Representation: All patients in the dataset are from the United States, and there are four unique geographical regions represented. Understanding the geographic distribution of patients helps contextualise the dataset within a specific country.

- Average Yearly Medical Insurance Charges: The average yearly medical insurance charge per individual is $13,270. This provides a baseline for understanding the typical cost of medical insurance within the dataset.

- Patient Data Dictionary: Organising the patient data into a dictionary format enhances its usability for further analysis. This structured approach facilitates easy access to individual patient information and supports continued investigations into various attributes.

## Further Research Suggestions:

- Attribute Correlation Analysis: Investigate the relationship between patient attributes (e.g., age, BMI, smoking status, number of children) and yearly medical insurance charges. Identify attributes that strongly influence insurance charges and explore potential correlations between them.

- Predictive Modeling: Build predictive models to forecast medical insurance charges based on patient attributes. Utilise machine learning algorithms such as regression or ensemble methods to predict insurance costs accurately.

- Demographic Analysis: Explore demographic trends within the dataset, such as age distributions across different regions or gender disparities in insurance charges. This analysis can provide valuable insights into healthcare disparities and inform targeted interventions.

- Risk Assessment: Evaluate the impact of patient attributes on insurance risk assessment. Determine which factors contribute to higher insurance risks and develop strategies to mitigate them effectively.

- Longitudinal Analysis: Conduct longitudinal studies to track changes in insurance charges over time and assess the long-term impact of patient attributes on healthcare costs. This analysis can help identify trends and patterns in healthcare utilisation and expenditure.

By addressing these research suggestions, one can gain a deeper understanding of the factors influencing medical insurance charges and make informed decisions regarding healthcare policy, pricing strategies, and risk management.
