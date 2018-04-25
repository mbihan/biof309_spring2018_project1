% Name Game Gender Prediction using Sound 
% Monika Bihan 
% Spring 2018

# Words can have different spelling but same sounds
- Monica Monika
- Use `fuzzy module` with `nysiis` method
- Takes a string and outputs a phonetic (that is, sound) version of that string
        - `fuzzy.nysiis('color') `
           and 
          `fuzzy.nysiis('colour')`
           both sound as **CALAR**

# Read author names on Children's Picture Books and get their gender
- Use `panda` module
- ## Extract first name of author from file and append it to the dataframe
  - Reading in datasets/nytkids_yearly.csv, which is semicolon delimited.
  
        author_df = pd.read_csv('datasets/nytkids_yearly.csv', sep=';')

  - Looping through author_df['Author'] to extract the authors first names
  
        first_name = []
        for name in author_df['Author']:
                first_name.append(name.split(' ')[0])

  - Adding first_name as a column to author_df
  
        author_df['first_name'] = first_name

   - First few lines of dataframe
        
                Year	Book Title	Author	Besteller this year	first_name
              0	2017	DRAGONS LOVE TACOS	Adam Rubin	49	Adam
              1	2017	THE WONDERFUL THINGS YOU WILL BE	Emily Winfield Martin	48	Emily
              2	2017	THE DAY THE CRAYONS QUIT	Drew Daywalt	44	Drew
              3	2017	ROSIE REVERE, ENGINEER	Andrea Beaty	38	Andrea
              4	2017	ADA TWIST, SCIENTIST	Andrea Beaty	28	Andrea

- ## Get the phenotic sounds on the first name of author list
  - Importing numpy
  
        import numpy as np

  - Looping through author's first names to create the nysiis (fuzzy) equivalent
  
        nysiis_name = []
        for first in author_df['first_name']:
                nysiis_name.append(fuzzy.nysiis(first))
    

   - Adding nysiis_name as a column to author_df
   
        author_df['nysiis_name'] = nysiis_name


   - First few lines of dataframe
   
                Year	Book Title	Author	Besteller this year	first_name	nysiis_name
              0	2017	DRAGONS LOVE TACOS	Adam Rubin	49	Adam	ADAN
              1	2017	THE WONDERFUL THINGS YOU WILL BE	Emily Winfield Martin	48	Emily	ENALY
              2	2017	THE DAY THE CRAYONS QUIT	Drew Daywalt	44	Drew	DR
              3	2017	ROSIE REVERE, ENGINEER	Andrea Beaty	38	Andrea	ANDR
              4	2017	ADA TWIST, SCIENTIST	Andrea Beaty	28	Andrea	ANDR

- ## Extract the gender information from Social Security Administartion baby name data
  - Reading in datasets/babynames_nysiis.csv, which is semicolon delimited
        
        babies_df = pd.read_csv('datasets/babynames_nysiis.csv', sep=';')

  - Looping through babies_df to and filling up gender
        
        gender = []
        num = 0
        for name in babies_df['babynysiis']:
                if(babies_df.at[num,'perc_female']>babies_df.at[num,'perc_male']):
                        gender.append('F')
                elif(babies_df.at[num,'perc_female']<babies_df.at[num,'perc_male']):
                        gender.append('M')
                else:
                        gender.append('N')
                num = num+1


   - Adding a gender column to babies_df
        
        babies_df['gender'] = gender
        
   - It has information on the percentage of times the name appeared as a female name 
        and the percentage of times it appeared as a male name
        
- ## Match the baby names to the author first names and get the author gender
  - This function returns the location of an element in a_list. Where an item does not exist, it returns -1
  
        def locate_in_list(a_list, element):
        loc_of_name = a_list.index(element) if element in a_list else -1
        return(loc_of_name)

  - Looping through author_df['nysiis_name'] and appending the gender of each. Author to author_gender
  
        author_gender = []
        for name in author_df['nysiis_name']:
                index_loc = locate_in_list(babies_df['babynysiis'].tolist(), name)
                if(index_loc == -1):
                        gender = 'Unknown'
                else:
                        gender = babies_df.at[index_loc, 'gender']
                author_gender.append(gender)

  - Adding author_gender to the author_df
  
        author_df['author_gender'] = author_gender

  - Counting the author's genders

        author_df['author_gender'].value_counts()
        
        F          395
        M          191
        Unknown      9
        N            8
        
    - Based on the counts we have more female authors on the New York Times best seller's list than male authors

# Analyze author gender distribution over time
 - Find number of male and female authors per year
 - Examine the changes over time
 
 - Creating a list of unique years, sorted in ascending order
 
        years = list(author_df['Year'].unique())
        years.sort(key=int)


 - Initializing lists
 
        males_by_yr = []
        females_by_yr = []
        unknown_by_yr = []

 - Looping through years to find the number of male, female and unknown authors per year
 
        for year in years:
                males_by_yr.append(len(author_df.loc[(author_df['Year'] == year) & (author_df['author_gender'] == 'M')]))
                females_by_yr.append(len(author_df.loc[(author_df['Year'] == year) & (author_df['author_gender'] == 'F')]))
                unknown_by_yr.append(len(author_df.loc[(author_df['Year'] == year) & (author_df['author_gender'] == 'Unknown')]))
 
 
# Explore unknown gender trend assuming them as foreign authors whose name not in the social security dataset
  - Importing matplotlib
  
        import matplotlib.pyplot as plt

  - This makes plots appear in the notebook
  
        %matplotlib inline

  - Plotting the bar chart
   
        fig = plt.figure()
        plt.bar(years,unknown_by_yr)

  - [OPTIONAL] - Setting a title, and axes label
  
        fig.suptitle('Unknown author by year')
        plt.show()

 ![alt text][Gbar]
   [Gbar]: https://github.com/mbihan/biof309_spring2018_project1/Name_Game__Gender_Prediction_using_Sound/img/yearBar.png "Year Bar Chart"

# Explore the distribution of female and male authors over time as grouped bar chart
  - Creating a new list, where 0.25 is added to each year
  
        years_shifted = [year + 0.25 for year in years]

  - Plotting males_by_yr by year
  
        plt.bar(years, males_by_yr, width=.25, color='lightblue')

  - Plotting females_by_yr by years_shifted
  
        plt.bar(years_shifted, females_by_yr, width=.25, color='pink')

  - [OPTIONAL] - Adding relevant Axes labels and Chart Title
  
        plt.xlabel = 'Years'
   
   ![alt text][Gbar]
   [Gbar]: https://github.com/mbihan/biof309_spring2018_project1/Name_Game__Gender_Prediction_using_Sound/img/groupedBar.png "Male and Female Bar Chart"

# Thanks
  - DataCamp
  - Prof Martin

# Questions

