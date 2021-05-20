# Data-Science-Intern
UNIT 2:
                                     
    {
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "![MLU Logo](../data/MLU_Logo.png)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <a name=\"0\">Machine Learning Accelerator - Tabular Data - Lecture 1</a>\n",
    "\n",
    "\n",
    "## Exploratory data analysis\n",
    "\n",
    "In this notebook, we go through basic steps of exploratory data analysis (EDA), performing initial data investigations to discover patterns, spot anomalies, and look for insights to inform later ML modeling choices.\n",
    "\n",
    "1. <a href=\"#1\">Read the dataset</a>\n",
    "2. <a href=\"#2\">Overall Statistics</a>\n",
    "3. <a href=\"#3\">Univariate Statistics: Basic Plots</a>\n",
    "4. <a href=\"#4\">Multivariate Statistics: Scatter Plots and Correlations</a>\n",
    "5. <a href=\"#5\">Handling Missing Values</a>\n",
    "    * <a href=\"#51\">Drop columns with missing values</a>\n",
    "    * <a href=\"#52\">Drop rows with missing values</a>\n",
    "    * <a href=\"#53\">Impute (fill-in) missing values with .fillna()</a>\n",
    "    * <a href=\"#54\">Impute (fill-in) missing values with sklearn's SimpleImputer</a>\n",
    "    \n",
    "__Austin Animal Center Dataset__:\n",
    "\n",
    "In this exercise, we are working with pet adoption data from __Austin Animal Center__. We have two datasets that cover intake and outcome of animals. Intake data is available from [here](https://data.austintexas.gov/Health-and-Community-Services/Austin-Animal-Center-Intakes/wter-evkm) and outcome is from [here](https://data.austintexas.gov/Health-and-Community-Services/Austin-Animal-Center-Outcomes/9t4d-g238). \n",
    "\n",
    "In order to work with a single table, we joined the intake and outcome tables using the \"Animal ID\" column and created a single __review.csv__ file. We also didn't consider animals with multiple entries to the facility to keep our dataset simple. If you want to see the original datasets and the merged data with multiple entries, they are available under data/review folder: Austin_Animal_Center_Intakes.csv, Austin_Animal_Center_Outcomes.csv and Austin_Animal_Center_Intakes_Outcomes.csv.\n",
    "\n",
    "__Dataset schema:__ \n",
    "- __Pet ID__ - Unique ID of pet\n",
    "- __Outcome Type__ - State of pet at the time of recording the outcome (0 = not placed, 1 = placed). This is the field to predict.\n",
    "- __Sex upon Outcome__ - Sex of pet at outcome\n",
    "- __Name__ - Name of pet \n",
    "- __Found Location__ - Found location of pet before entered the center\n",
    "- __Intake Type__ - Circumstances bringing the pet to the center\n",
    "- __Intake Condition__ - Health condition of pet when entered the center\n",
    "- __Pet Type__ - Type of pet\n",
    "- __Sex upon Intake__ - Sex of pet when entered the center\n",
    "- __Breed__ - Breed of pet \n",
    "- __Color__ - Color of pet \n",
    "- __Age upon Intake Days__ - Age of pet when entered the center (days)\n",
    "- __Age upon Outcome Days__ - Age of pet at outcome (days)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 1. <a name=\"1\">Read the dataset</a>\n",
    "(<a href=\"#0\">Go to top</a>)\n",
    "\n",
    "Let's read the dataset into a dataframe, using Pandas."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The shape of the dataset is: (95485, 13)\n"
     ]
    }
   ],
   "source": [
    "import pandas as pd\n",
    "\n",
    "import warnings\n",
    "warnings.filterwarnings(\"ignore\")\n",
    "  \n",
    "df = pd.read_csv('../data/review/review_dataset.csv')\n",
    "\n",
    "print('The shape of the dataset is:', df.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 2. <a name=\"2\">Overall Statistics</a>\n",
    "(<a href=\"#0\">Go to top</a>)\n",
    "\n",
    "We will look at number of rows, columns and some simple statistics of the dataset."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Pet ID</th>\n",
       "      <th>Outcome Type</th>\n",
       "      <th>Sex upon Outcome</th>\n",
       "      <th>Name</th>\n",
       "      <th>Found Location</th>\n",
       "      <th>Intake Type</th>\n",
       "      <th>Intake Condition</th>\n",
       "      <th>Pet Type</th>\n",
       "      <th>Sex upon Intake</th>\n",
       "      <th>Breed</th>\n",
       "      <th>Color</th>\n",
       "      <th>Age upon Intake Days</th>\n",
       "      <th>Age upon Outcome Days</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>A794011</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>Chunk</td>\n",
       "      <td>Austin (TX)</td>\n",
       "      <td>Owner Surrender</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Brown Tabby/White</td>\n",
       "      <td>730</td>\n",
       "      <td>730</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>A776359</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>Gizmo</td>\n",
       "      <td>7201 Levander Loop in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Dog</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Chihuahua Shorthair Mix</td>\n",
       "      <td>White/Brown</td>\n",
       "      <td>365</td>\n",
       "      <td>365</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>A674754</td>\n",
       "      <td>0.0</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>NaN</td>\n",
       "      <td>12034 Research in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Nursing</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Orange Tabby</td>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>A689724</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>*Donatello</td>\n",
       "      <td>2300 Waterway Bnd in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Black</td>\n",
       "      <td>60</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>A680969</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>*Zeus</td>\n",
       "      <td>4701 Staggerbrush Rd in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Nursing</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>White/Orange Tabby</td>\n",
       "      <td>7</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Pet ID  Outcome Type Sex upon Outcome        Name  \\\n",
       "0  A794011           1.0    Neutered Male       Chunk   \n",
       "1  A776359           1.0    Neutered Male       Gizmo   \n",
       "2  A674754           0.0      Intact Male         NaN   \n",
       "3  A689724           1.0    Neutered Male  *Donatello   \n",
       "4  A680969           1.0    Neutered Male       *Zeus   \n",
       "\n",
       "                        Found Location      Intake Type Intake Condition  \\\n",
       "0                          Austin (TX)  Owner Surrender           Normal   \n",
       "1    7201 Levander Loop in Austin (TX)            Stray           Normal   \n",
       "2        12034 Research in Austin (TX)            Stray          Nursing   \n",
       "3     2300 Waterway Bnd in Austin (TX)            Stray           Normal   \n",
       "4  4701 Staggerbrush Rd in Austin (TX)            Stray          Nursing   \n",
       "\n",
       "  Pet Type Sex upon Intake                    Breed               Color  \\\n",
       "0      Cat   Neutered Male   Domestic Shorthair Mix   Brown Tabby/White   \n",
       "1      Dog     Intact Male  Chihuahua Shorthair Mix         White/Brown   \n",
       "2      Cat     Intact Male   Domestic Shorthair Mix        Orange Tabby   \n",
       "3      Cat     Intact Male   Domestic Shorthair Mix               Black   \n",
       "4      Cat     Intact Male   Domestic Shorthair Mix  White/Orange Tabby   \n",
       "\n",
       "   Age upon Intake Days  Age upon Outcome Days  \n",
       "0                   730                    730  \n",
       "1                   365                    365  \n",
       "2                     6                      6  \n",
       "3                    60                     60  \n",
       "4                     7                     60  "
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Print the first five rows\n",
    "# NaN means missing data\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 95485 entries, 0 to 95484\n",
      "Data columns (total 13 columns):\n",
      "Pet ID                   95485 non-null object\n",
      "Outcome Type             95485 non-null float64\n",
      "Sex upon Outcome         95484 non-null object\n",
      "Name                     59138 non-null object\n",
      "Found Location           95485 non-null object\n",
      "Intake Type              95485 non-null object\n",
      "Intake Condition         95485 non-null object\n",
      "Pet Type                 95485 non-null object\n",
      "Sex upon Intake          95484 non-null object\n",
      "Breed                    95485 non-null object\n",
      "Color                    95485 non-null object\n",
      "Age upon Intake Days     95485 non-null int64\n",
      "Age upon Outcome Days    95485 non-null int64\n",
      "dtypes: float64(1), int64(2), object(10)\n",
      "memory usage: 9.5+ MB\n"
     ]
    }
   ],
   "source": [
    "# Let's see the data types and non-null values for each column\n",
    "df.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Outcome Type</th>\n",
       "      <th>Age upon Intake Days</th>\n",
       "      <th>Age upon Outcome Days</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>95485.000000</td>\n",
       "      <td>95485.000000</td>\n",
       "      <td>95485.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>0.564005</td>\n",
       "      <td>703.436959</td>\n",
       "      <td>717.757313</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>0.495889</td>\n",
       "      <td>1052.252197</td>\n",
       "      <td>1055.023160</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>30.000000</td>\n",
       "      <td>60.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>365.000000</td>\n",
       "      <td>365.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>9125.000000</td>\n",
       "      <td>9125.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       Outcome Type  Age upon Intake Days  Age upon Outcome Days\n",
       "count  95485.000000          95485.000000           95485.000000\n",
       "mean       0.564005            703.436959             717.757313\n",
       "std        0.495889           1052.252197            1055.023160\n",
       "min        0.000000              0.000000               0.000000\n",
       "25%        0.000000             30.000000              60.000000\n",
       "50%        1.000000            365.000000             365.000000\n",
       "75%        1.000000            730.000000             730.000000\n",
       "max        1.000000           9125.000000            9125.000000"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# This prints basic statistics for numerical columns\n",
    "df.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's separate model features and model target."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Index(['Pet ID', 'Outcome Type', 'Sex upon Outcome', 'Name', 'Found Location',\n",
      "       'Intake Type', 'Intake Condition', 'Pet Type', 'Sex upon Intake',\n",
      "       'Breed', 'Color', 'Age upon Intake Days', 'Age upon Outcome Days'],\n",
      "      dtype='object')\n"
     ]
    }
   ],
   "source": [
    "print(df.columns)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Model features:  Index(['Pet ID', 'Sex upon Outcome', 'Name', 'Found Location', 'Intake Type',\n",
      "       'Intake Condition', 'Pet Type', 'Sex upon Intake', 'Breed', 'Color',\n",
      "       'Age upon Intake Days', 'Age upon Outcome Days'],\n",
      "      dtype='object')\n",
      "Model target:  Outcome Type\n"
     ]
    }
   ],
   "source": [
    "model_features = df.columns.drop('Outcome Type')\n",
    "model_target = 'Outcome Type'\n",
    "\n",
    "print('Model features: ', model_features)\n",
    "print('Model target: ', model_target)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can explore the features set further, figuring out first what features are numerical or categorical. Beware that some integer-valued features could actually be categorical features, and some categorical features could be text features. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Numerical columns: Index(['Age upon Intake Days', 'Age upon Outcome Days'], dtype='object')\n",
      "\n",
      "Categorical columns: Index(['Pet ID', 'Sex upon Outcome', 'Name', 'Found Location', 'Intake Type',\n",
      "       'Intake Condition', 'Pet Type', 'Sex upon Intake', 'Breed', 'Color'],\n",
      "      dtype='object')\n"
     ]
    }
   ],
   "source": [
    "import numpy as np\n",
    "numerical_features_all = df[model_features].select_dtypes(include=np.number).columns\n",
    "print('Numerical columns:',numerical_features_all)\n",
    "\n",
    "print('')\n",
    "\n",
    "categorical_features_all = df[model_features].select_dtypes(include='object').columns\n",
    "print('Categorical columns:',categorical_features_all)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 3. <a name=\"3\">Basic Plots</a>\n",
    "(<a href=\"#0\">Go to top</a>)\n",
    "\n",
    "In this section, we examine our data with plots. Important note: These plots ignore null (missing) values. We will learn how to deal with missing values in the next section.\n",
    "\n",
    "\n",
    "__Bar plots__: These plots show counts of categorical data fields. __value_counts()__ function yields the counts of each unique value. It is useful for categorical variables.\n",
    "\n",
    "First, let's look at the distribution of the model target."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1.0    53854\n",
       "0.0    41631\n",
       "Name: Outcome Type, dtype: int64"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df[model_target].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "__plot.bar()__ addition to the __value_counts()__ function makes a bar plot of the values."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "%matplotlib inline\n",
    "\n",
    "df[model_target].value_counts().plot.bar()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now onto the categorical features, exploring number of unique values per feature."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "A811059    1\n",
      "A729450    1\n",
      "A765406    1\n",
      "A692504    1\n",
      "A488203    1\n",
      "          ..\n",
      "A759023    1\n",
      "A704896    1\n",
      "A790700    1\n",
      "A777953    1\n",
      "A814738    1\n",
      "Name: Pet ID, Length: 95485, dtype: int64\n",
      "Neutered Male    30244\n",
      "Spayed Female    28145\n",
      "Intact Female    13724\n",
      "Intact Male      13646\n",
      "Unknown           9725\n",
      "Name: Sex upon Outcome, dtype: int64\n",
      "Bella     338\n",
      "Luna      313\n",
      "Max       311\n",
      "Daisy     239\n",
      "Lucy      223\n",
      "         ... \n",
      "*Tulla      1\n",
      "Phobe       1\n",
      "Arti        1\n",
      "Oreto       1\n",
      "Wildo       1\n",
      "Name: Name, Length: 17468, dtype: int64\n",
      "Austin (TX)                               14833\n",
      "Travis (TX)                                1402\n",
      "7201 Levander Loop in Austin (TX)           644\n",
      "Outside Jurisdiction                        607\n",
      "Del Valle (TX)                              426\n",
      "                                          ...  \n",
      "5503 Tallow Tree Dr in Austin (TX)            1\n",
      "Burnet Rd & Hancock Dr in Austin (TX)         1\n",
      "7104 Berman Dr in Austin (TX)                 1\n",
      "9300 East Hunters Trace in Austin (TX)        1\n",
      "1304 Mariposa in Austin (TX)                  1\n",
      "Name: Found Location, Length: 43951, dtype: int64\n",
      "Stray                 70203\n",
      "Owner Surrender       15146\n",
      "Public Assist          5236\n",
      "Wildlife               4554\n",
      "Euthanasia Request      235\n",
      "Abandoned               111\n",
      "Name: Intake Type, dtype: int64\n",
      "Normal      81912\n",
      "Injured      5386\n",
      "Sick         4291\n",
      "Nursing      3172\n",
      "Aged          352\n",
      "Other         189\n",
      "Feral          97\n",
      "Pregnant       63\n",
      "Medical        21\n",
      "Behavior        2\n",
      "Name: Intake Condition, dtype: int64\n",
      "Dog          48719\n",
      "Cat          40082\n",
      "Other         6115\n",
      "Bird           553\n",
      "Livestock       16\n",
      "Name: Pet Type, dtype: int64\n",
      "Intact Male      33369\n",
      "Intact Female    32515\n",
      "Neutered Male    10521\n",
      "Unknown           9725\n",
      "Spayed Female     9354\n",
      "Name: Sex upon Intake, dtype: int64\n",
      "Domestic Shorthair Mix              27689\n",
      "Domestic Shorthair                   5076\n",
      "Pit Bull Mix                         5017\n",
      "Chihuahua Shorthair Mix              4963\n",
      "Labrador Retriever Mix               4789\n",
      "                                    ...  \n",
      "Rhod Ridgeback/German Shepherd          1\n",
      "Mastiff/Boxer                           1\n",
      "Cocker Spaniel/Shetland Sheepdog        1\n",
      "Basenji/Carolina Dog                    1\n",
      "Bluebird                                1\n",
      "Name: Breed, Length: 2395, dtype: int64\n",
      "Black/White               9688\n",
      "Black                     8528\n",
      "Brown Tabby               6077\n",
      "Brown                     4440\n",
      "White                     3312\n",
      "                          ... \n",
      "Liver Tick/White             1\n",
      "Chocolate/Gray               1\n",
      "Tortie/Tortie                1\n",
      "Tricolor/Brown Brindle       1\n",
      "Gray Tabby/Orange            1\n",
      "Name: Color, Length: 567, dtype: int64\n"
     ]
    }
   ],
   "source": [
    "for c in categorical_features_all: \n",
    "    print(df[c].value_counts())\n",
    "    "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Based on the number of unique values (unique IDs for example won't be very useful to visualize, for example), for some categorical features, let's see some bar plot visualizations. For simplicity and speed, here we only show box plots for those features with less than 50 unique values."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Sex upon Outcome\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Intake Type\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Intake Condition\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Pet Type\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Sex upon Intake\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "%matplotlib inline\n",
    "\n",
    "for c in categorical_features_all:\n",
    "    if len(df[c].value_counts()) < 50:\n",
    "        print(c)\n",
    "        df[c].value_counts().plot.bar()\n",
    "        plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "__Histograms:__ Histograms show distribution of numeric data. Data is divided into \"buckets\" or \"bins\"."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n"
     ]
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAZEAAAD4CAYAAAAtrdtxAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjMsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy+AADFEAAAaK0lEQVR4nO3df5Qd5X3f8ffHkgWCGCTBQrEkIhFviWVOAbEFJaSpg2wh4RTRHmjl+lRbqmRTIho77jmJSHoqB0wLPa5x1Tg4iiUjUdtCyD9QbRF1LXCangNCy48AQhCtgUhrKWjjFQKbGCz72z/mu+iyurt7mWXuctnP65w5M/Od55n7zHjE1zPPszOKCMzMzMp413g3wMzMWpeTiJmZleYkYmZmpTmJmJlZaU4iZmZW2uTxbkCznX766TFnzpzxboaZWct4+OGH/y4i2uptm3BJZM6cOfT09Ix3M8zMWoakvxlumx9nmZlZaU4iZmZWmpOImZmV5iRiZmalOYmYmVlpTiJmZlaak4iZmZXmJGJmZqU5iZiZWWkT7i/Wx2LOqm+PdxOa7vlbPjLeTTCztzHfiZiZWWlOImZmVlqlSUTS70raLelJSV+VdKKkuZJ2Stor6S5JU7LsCbnem9vn1Oznhow/I+nymvjijPVKWlXlsZiZ2fEqSyKSZgK/A3RExHnAJGAZcCtwW0S0A4eBFVllBXA4It4H3JblkDQv630AWAz8iaRJkiYBnweWAPOAj2ZZMzNrkqofZ00GpkqaDJwEHAQuA7bk9g3AVbm8NNfJ7QslKeObIuLViHgO6AUuzqk3Ip6NiNeATVnWzMyapLIkEhHfBz4D7KNIHkeAh4EXI+JoFusDZubyTGB/1j2a5U+rjQ+pM1zczMyapMrHWdMp7gzmAu8FTqZ49DRUDFYZZtubjddrS5ekHkk9/f39ozXdzMwaVOXjrA8Bz0VEf0T8BPg68MvAtHy8BTALOJDLfcBsgNx+KjBQGx9SZ7j4cSJibUR0RERHW1vdLzyamVkJVSaRfcACSSdl38ZC4CngfuDqLNMJ3JPLW3Od3H5fRETGl+XorblAO/AQsAtoz9FeUyg637dWeDxmZjZEZX+xHhE7JW0BHgGOAo8Ca4FvA5skfTpj67LKOuBOSb0UdyDLcj+7JW2mSEBHgZUR8VMASdcD2ylGfq2PiN1VHY+ZmR2v0teeRMRqYPWQ8LMUI6uGlv0xcM0w+7kZuLlOfBuwbewtNTOzMvwX62ZmVpqTiJmZleYkYmZmpTmJmJlZaU4iZmZWmpOImZmV5iRiZmalOYmYmVlpTiJmZlaak4iZmZXmJGJmZqU5iZiZWWlOImZmVpqTiJmZleYkYmZmpTmJmJlZaZUlEUnnSnqsZnpJ0ickzZDULWlvzqdneUlaI6lX0uOS5tfsqzPL75XUWRO/SNITWWdNfobXzMyapLIkEhHPRMQFEXEBcBHwCvANYBWwIyLagR25DrCE4vvp7UAXcDuApBkUX0e8hOKLiKsHE0+W6aqpt7iq4zEzs+M163HWQuB7EfE3wFJgQ8Y3AFfl8lJgYxQeBKZJOgu4HOiOiIGIOAx0A4tz2ykR8UBEBLCxZl9mZtYEzUoiy4Cv5vKZEXEQIOdnZHwmsL+mTl/GRor31YmbmVmTVJ5EJE0BrgTuHq1onViUiNdrQ5ekHkk9/f39ozTDzMwa1Yw7kSXAIxHxQq6/kI+iyPmhjPcBs2vqzQIOjBKfVSd+nIhYGxEdEdHR1tY2xsMxM7NBzUgiH+XYoyyArcDgCKtO4J6a+PIcpbUAOJKPu7YDiyRNzw71RcD23PaypAU5Kmt5zb7MzKwJJle5c0knAR8GfqsmfAuwWdIKYB9wTca3AVcAvRQjua4FiIgBSTcBu7LcjRExkMvXAXcAU4F7czIzsyapNIlExCvAaUNiP6AYrTW0bAArh9nPemB9nXgPcN5b0lgzM3vT/BfrZmZWmpOImZmV5iRiZmalOYmYmVlpTiJmZlaak4iZmZXmJGJmZqU5iZiZWWlOImZmVpqTiJmZleYkYmZmpTmJmJlZaU4iZmZWmpOImZmV5iRiZmalOYmYmVlplSYRSdMkbZH0tKQ9kn5J0gxJ3ZL25nx6lpWkNZJ6JT0uaX7Nfjqz/F5JnTXxiyQ9kXXW5GdyzcysSaq+E/kfwJ9HxC8C5wN7gFXAjohoB3bkOsASoD2nLuB2AEkzgNXAJcDFwOrBxJNlumrqLa74eMzMrEZlSUTSKcCvAusAIuK1iHgRWApsyGIbgKtyeSmwMQoPAtMknQVcDnRHxEBEHAa6gcW57ZSIeCA/rbuxZl9mZtYEVd6JnAP0A1+S9KikL0o6GTgzIg4C5PyMLD8T2F9Tvy9jI8X76sTNzKxJqkwik4H5wO0RcSHwI449uqqnXn9GlIgfv2OpS1KPpJ7+/v6RW21mZg2rMon0AX0RsTPXt1AklRfyURQ5P1RTfnZN/VnAgVHis+rEjxMRayOiIyI62traxnRQZmZ2TGVJJCL+Ftgv6dwMLQSeArYCgyOsOoF7cnkrsDxHaS0AjuTjru3AIknTs0N9EbA9t70saUGOylpesy8zM2uCyRXv/z8AX5Y0BXgWuJYicW2WtALYB1yTZbcBVwC9wCtZlogYkHQTsCvL3RgRA7l8HXAHMBW4NyczM2uSSpNIRDwGdNTZtLBO2QBWDrOf9cD6OvEe4LwxNtPMzEryX6ybmVlpTiJmZlaak4iZmZXmJGJmZqU5iZiZWWlOImZmVpqTiJmZleYkYmZmpTmJmJlZaU4iZmZWmpOImZmV5iRiZmalNZREJPklh2ZmdpxG70S+IOkhSb8taVqlLTIzs5bRUBKJiF8BPkbxhcEeSV+R9OFKW2ZmZm97DfeJRMRe4D8Bvw/8U2CNpKcl/YuqGmdmZm9vjfaJ/CNJtwF7gMuAfxYR78/l20ao97ykJyQ9JqknYzMkdUvam/PpGZekNZJ6JT0uaX7Nfjqz/F5JnTXxi3L/vVlXpc6CmZmV0uidyB8DjwDnR8TKiHgEICIOUNydjOTXIuKCiBj8wuEqYEdEtAM7ch1gCdCeUxdwOxRJB1gNXAJcDKweTDxZpqum3uIGj8fMzN4CjSaRK4CvRMTfA0h6l6STACLizjf5m0uBDbm8AbiqJr4xCg8C0ySdBVwOdEfEQEQcBrqBxbntlIh4ID+tu7FmX2Zm1gSNJpHvAFNr1k/K2GgC+D+SHpbUlbEzI+IgQM7PyPhMYH9N3b6MjRTvqxM3M7MmmdxguRMj4oeDKxHxw8E7kVFcGhEHJJ0BdEt6eoSy9fozokT8+B0XCawL4Oyzzx65xWZm1rBG70R+NKSj+yLg70erlH0mRMQh4BsUfRov5KMocn4oi/dRDCEeNAs4MEp8Vp14vXasjYiOiOhoa2sbrdlmZtagRpPIJ4C7Jf2lpL8E7gKuH6mCpJMlvWdwGVgEPAlsBQZHWHUC9+TyVmB5jtJaABzJx13bgUWSpmeH+iJge257WdKCHJW1vGZfZmbWBA09zoqIXZJ+ETiX4jHS0xHxk1GqnQl8I0fdTqbomP9zSbuAzZJWAPuAa7L8NooO/F7gFeDa/O0BSTcBu7LcjRExkMvXAXdQ9Nfcm5OZmTVJo30iAP8YmJN1LpRERGwcrnBEPAucXyf+A2BhnXgAK4fZ13pgfZ14D+D3epmZjZOGkoikO4FfAB4DfprhwWG1ZmY2QTV6J9IBzMu7BTMzM6DxjvUngX9QZUPMzKz1NHoncjrwlKSHgFcHgxFxZSWtMjOzltBoEvlUlY0wM7PW1OgQ37+Q9PNAe0R8J/9afVK1TTMzs7e7Rl8F/5vAFuBPMzQT+GZVjTIzs9bQaMf6SuBS4CV4/QNVZ4xYw8zM3vEaTSKvRsRrgyuSJjPMyw7NzGziaDSJ/IWkPwCm5rfV7wb+d3XNMjOzVtBoElkF9ANPAL9F8Z6r0b5oaGZm73CNjs76GfBnOZmZmQGNvzvrOer0gUTEOW95i8zMrGW8mXdnDTqR4vXtM9765piZWStpqE8kIn5QM30/Ij4HXFZx28zM7G2u0cdZ82tW30VxZ/KeSlpkZmYto9HRWf+9ZvqvwEXAv2ykoqRJkh6V9K1cnytpp6S9ku6SNCXjJ+R6b26fU7OPGzL+jKTLa+KLM9YraVWDx2JmZm+RRkdn/doYfuPjwB7glFy/FbgtIjZJ+gKwArg954cj4n2SlmW5fyVpHrAM+ADwXuA7kv5h7uvzwIeBPmCXpK0R8dQY2mpmZm9Co4+zPjnS9oj47DD1ZgEfAW4GPqnig+uXAf86i2ygeEPw7cBSjr0teAvwx1l+KbApIl4FnpPUC1yc5XrzM7xI2pRlnUTMzJqk0cdZHcB1FC9enAn8e2AeRb/ISH0jnwN+D/hZrp8GvBgRR3O9L/dHzvcD5PYjWf71+JA6w8XNzKxJ3sxHqeZHxMsAkj4F3B0RvzFcBUm/DhyKiIclfXAwXKdojLJtuHi9BFj3fV6SuoAugLPPPnu4JpuZ2ZvU6J3I2cBrNeuvAXNGqXMpcKWk54FNFI+xPgdMyxc4AswCDuRyHzAbXn/B46nAQG18SJ3h4seJiLUR0RERHW1tbaM028zMGtVoErkTeEjSpyStBnYCG0eqEBE3RMSsiJhD0TF+X0R8DLgfuDqLdQL35PLWXCe33xcRkfFlOXprLtAOPATsAtpztNeU/I2tDR6PmZm9BRodnXWzpHuBf5KhayPi0ZK/+fvAJkmfBh4F1mV8HXBndpwPUCQFImK3pM0UHeZHgZUR8VMASdcD2ym+srg+InaXbJOZmZXQaJ8IwEnASxHxJUltkuZGxHONVIyI7wLfzeVnOTa6qrbMjylep1Kv/s0UI7yGxrdRvFHYzMzGQaOfx11NcQdxQ4beDfyvqhplZmatodE+kX8OXAn8CCAiDuDXnpiZTXiNJpHXspM7ACSdXF2TzMysVTSaRDZL+lOK4bm/CXwHf6DKzGzCa3R01mfy2+ovAecC/zkiuittmZmZve2NmkQkTQK2R8SHACcOMzN73aiPs/JvMl6RdGoT2mNmZi2k0b8T+THwhKRucoQWQET8TiWtMjOzltBoEvl2TmZmZq8bMYlIOjsi9kXEhmY1yMzMWsdofSLfHFyQ9LWK22JmZi1mtCRS+y2Pc6psiJmZtZ7RkkgMs2xmZjZqx/r5kl6iuCOZmsvkekTEKZW2zszM3tZGTCIRMalZDTEzs9bT6LuzzMzMjuMkYmZmpVWWRCSdKOkhSX8labekP8r4XEk7Je2VdFd+H538hvpdknpz+5yafd2Q8WckXV4TX5yxXkmrqjoWMzOrr8o7kVeByyLifOACYLGkBcCtwG0R0Q4cBlZk+RXA4Yh4H3BblkPSPIrvrX8AWAz8iaRJ+WLIzwNLgHnAR7OsmZk1SWVJJAo/zNV35xTAZcCWjG8ArsrlpblObl8oSRnfFBGv5jfdeym+0X4x0BsRz0bEa8CmLGtmZk1SaZ9I3jE8BhyieI3894AXI+JoFukDZubyTGA/QG4/ApxWGx9SZ7h4vXZ0SeqR1NPf3/9WHJqZmVFxEomIn0bEBcAsijuH99crlnMNs+3Nxuu1Y21EdERER1tb2+gNNzOzhjRldFZEvAh8F1hA8Yndwb9PmQUcyOU+YDZAbj8VGKiND6kzXNzMzJqkytFZbZKm5fJU4EPAHuB+4Oos1gnck8tbc53cfl9ERMaX5eituUA78BCwC2jP0V5TKDrft1Z1PGZmdrxGvydSxlnAhhxF9S5gc0R8S9JTwCZJnwYeBdZl+XXAnZJ6Ke5AlgFExG5Jm4GngKPAyvzaIpKuB7YDk4D1EbG7wuMxM7MhKksiEfE4cGGd+LMU/SND4z8GrhlmXzcDN9eJbwO2jbmxZmZWiv9i3czMSnMSMTOz0pxEzMysNCcRMzMrzUnEzMxKcxIxM7PSnETMzKw0JxEzMyvNScTMzEpzEjEzs9KcRMzMrDQnETMzK81JxMzMSnMSMTOz0pxEzMysNCcRMzMrrcrP486WdL+kPZJ2S/p4xmdI6pa0N+fTMy5JayT1Snpc0vyafXVm+b2SOmviF0l6IuuskaSqjsfMzI5X5Z3IUeA/RsT7gQXASknzgFXAjohoB3bkOsASiu+ntwNdwO1QJB1gNXAJxRcRVw8mnizTVVNvcYXHY2ZmQ1SWRCLiYEQ8kssvA3uAmcBSYEMW2wBclctLgY1ReBCYJuks4HKgOyIGIuIw0A0szm2nRMQDERHAxpp9mZlZEzSlT0TSHIrvre8EzoyIg1AkGuCMLDYT2F9TrS9jI8X76sTr/X6XpB5JPf39/WM9HDMzS5UnEUk/B3wN+EREvDRS0TqxKBE/PhixNiI6IqKjra1ttCabmVmDKk0ikt5NkUC+HBFfz/AL+SiKnB/KeB8wu6b6LODAKPFZdeJmZtYkVY7OErAO2BMRn63ZtBUYHGHVCdxTE1+eo7QWAEfycdd2YJGk6dmhvgjYnttelrQgf2t5zb7MzKwJJle470uBfwM8IemxjP0BcAuwWdIKYB9wTW7bBlwB9AKvANcCRMSApJuAXVnuxogYyOXrgDuAqcC9OZmZWZNUlkQi4v9Rv98CYGGd8gGsHGZf64H1deI9wHljaKaZmY2B/2LdzMxKcxIxM7PSnETMzKw0JxEzMyvNScTMzEpzEjEzs9KcRMzMrDQnETMzK81JxMzMSnMSMTOz0pxEzMysNCcRMzMrzUnEzMxKcxIxM7PSnETMzKw0JxEzMyutys/jrpd0SNKTNbEZkrol7c359IxL0hpJvZIelzS/pk5nlt8rqbMmfpGkJ7LOmvxErpmZNVGVdyJ3AIuHxFYBOyKiHdiR6wBLgPacuoDboUg6wGrgEuBiYPVg4skyXTX1hv6WmZlVrLIkEhH/FxgYEl4KbMjlDcBVNfGNUXgQmCbpLOByoDsiBiLiMNANLM5tp0TEA/lZ3Y01+zIzsyZpdp/ImRFxECDnZ2R8JrC/plxfxkaK99WJ1yWpS1KPpJ7+/v4xH4SZmRXeLh3r9fozokS8rohYGxEdEdHR1tZWsolmZjZUs5PIC/koipwfyngfMLum3CzgwCjxWXXiZmbWRJOb/HtbgU7glpzfUxO/XtImik70IxFxUNJ24L/UdKYvAm6IiAFJL0taAOwElgP/s5kHMlHMWfXt8W5C0z1/y0fGuwlmLaOyJCLpq8AHgdMl9VGMsroF2CxpBbAPuCaLbwOuAHqBV4BrATJZ3ATsynI3RsRgZ/11FCPApgL35mRmZk1UWRKJiI8Os2lhnbIBrBxmP+uB9XXiPcB5Y2mjmZmNzdulY93MzFqQk4iZmZXmJGJmZqU5iZiZWWlOImZmVpqTiJmZleYkYmZmpTmJmJlZaU4iZmZWmpOImZmV1uwXMJq97U20l076hZM2Fr4TMTOz0pxEzMysNCcRMzMrzUnEzMxKcxIxM7PSWj6JSFos6RlJvZJWjXd7zMwmkpYe4itpEvB54MNAH7BL0taIeGp8W2bWOibakGbwsOa3UqvfiVwM9EbEsxHxGrAJWDrObTIzmzBa+k4EmAnsr1nvAy4ZWkhSF9CVqz+U9EzJ3zsd+LuSdd9pfC4KPg/HtMy50K2V/0TLnIsG/fxwG1o9iahOLI4LRKwF1o75x6SeiOgY637eCXwuCj4Px/hcHDORzkWrP87qA2bXrM8CDoxTW8zMJpxWTyK7gHZJcyVNAZYBW8e5TWZmE0ZLP86KiKOSrge2A5OA9RGxu8KfHPMjsXcQn4uCz8MxPhfHTJhzoYjjuhDMzMwa0uqPs8zMbBw5iZiZWWlOIg2YCK9WkTRb0v2S9kjaLenjGZ8hqVvS3pxPz7gkrclz8rik+TX76szyeyV1jtcxjYWkSZIelfStXJ8raWce0105kANJJ+R6b26fU7OPGzL+jKTLx+dIxkbSNElbJD2d18YvTeBr4nfz38aTkr4q6cSJel28QUR4GmGi6LD/HnAOMAX4K2DeeLerguM8C5ify+8B/hqYB/w3YFXGVwG35vIVwL0Uf6uzANiZ8RnAszmfnsvTx/v4SpyPTwJfAb6V65uBZbn8BeC6XP5t4Au5vAy4K5fn5bVyAjA3r6FJ431cJc7DBuA3cnkKMG0iXhMUf9j8HDC15nr4txP1uqidfCcyugnxapWIOBgRj+Tyy8Aein84Syn+Q0LOr8rlpcDGKDwITJN0FnA50B0RAxFxGOgGFjfxUMZM0izgI8AXc13AZcCWLDL0PAyeny3Awiy/FNgUEa9GxHNAL8W11DIknQL8KrAOICJei4gXmYDXRJoMTJU0GTgJOMgEvC6GchIZXb1Xq8wcp7Y0Rd56XwjsBM6MiINQJBrgjCw23Hl5J5yvzwG/B/ws108DXoyIo7lee0yvH29uP5Ll3wnn4RygH/hSPtr7oqSTmYDXRER8H/gMsI8ieRwBHmZiXhdv4CQyuoZerfJOIenngK8Bn4iIl0YqWicWI8RbgqRfBw5FxMO14TpFY5RtLX0e0mRgPnB7RFwI/Iji8dVw3rHnIvt9llI8gnovcDKwpE7RiXBdvIGTyOgmzKtVJL2bIoF8OSK+nuEX8pEEOT+U8eHOS6ufr0uBKyU9T/Ho8jKKO5Np+RgD3nhMrx9vbj8VGKD1zwMUx9AXETtzfQtFUplo1wTAh4DnIqI/In4CfB34ZSbmdfEGTiKjmxCvVsnnteuAPRHx2ZpNW4HB0TSdwD018eU5ImcBcCQfbWwHFkmanv/vbVHGWkJE3BARsyJiDsX/1vdFxMeA+4Grs9jQ8zB4fq7O8pHxZTlKZy7QDjzUpMN4S0TE3wL7JZ2boYXAU0ywayLtAxZIOin/rQyeiwl3XRxnvHv2W2GiGHXy1xQjKf5wvNtT0TH+CsVt9ePAYzldQfEcdwewN+czsrwoPgj2PeAJoKNmX/+OosOwF7h2vI9tDOfkgxwbnXUOxT/2XuBu4ISMn5jrvbn9nJr6f5jn5xlgyXgfT8lzcAHQk9fFNylGV03IawL4I+Bp4EngTooRVhPyuqid/NoTMzMrzY+zzMysNCcRMzMrzUnEzMxKcxIxM7PSnETMzKw0JxEzMyvNScTMzEr7/9PCGWutJwR7AAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Outcome Days\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "%matplotlib inline\n",
    "\n",
    "for c in numerical_features_all:\n",
    "    print(c)\n",
    "    df[c].plot.hist(bins=5)\n",
    "    plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If for some histograms the values are heavily placed in the first bin, it is good to check for outliers, either checking the min-max values of those particular features and/or explore value ranges."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n",
      "min: 0 max: 9125\n",
      "Age upon Outcome Days\n",
      "min: 0 max: 9125\n"
     ]
    }
   ],
   "source": [
    "for c in numerical_features_all:\n",
    "    print(c)\n",
    "    print('min:', df[c].min(), 'max:', df[c].max())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "With __value_counts()__ function, we can increase the number of histogram bins to 10 for more bins for a more refined view of the numerical features."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n",
      "(-9.126, 912.5]     74835\n",
      "(912.5, 1825.0]     10647\n",
      "(1825.0, 2737.5]     3471\n",
      "(2737.5, 3650.0]     3998\n",
      "(3650.0, 4562.5]     1234\n",
      "(4562.5, 5475.0]     1031\n",
      "(5475.0, 6387.5]      183\n",
      "(6387.5, 7300.0]       79\n",
      "(7300.0, 8212.5]        5\n",
      "(8212.5, 9125.0]        2\n",
      "Name: Age upon Intake Days, dtype: int64\n",
      "Age upon Outcome Days\n",
      "(-9.126, 912.5]     74642\n",
      "(912.5, 1825.0]     10699\n",
      "(1825.0, 2737.5]     3465\n",
      "(2737.5, 3650.0]     4080\n",
      "(3650.0, 4562.5]     1263\n",
      "(4562.5, 5475.0]     1061\n",
      "(5475.0, 6387.5]      187\n",
      "(6387.5, 7300.0]       81\n",
      "(7300.0, 8212.5]        5\n",
      "(8212.5, 9125.0]        2\n",
      "Name: Age upon Outcome Days, dtype: int64\n"
     ]
    }
   ],
   "source": [
    "for c in numerical_features_all: \n",
    "    print(c)\n",
    "    print(df[c].value_counts(bins=10, sort=False))\n",
    "    plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If any outliers are identified as very likely wrong values, dropping them could improve the numerical values histograms, and later overall model performance. While a good rule of thumb is that anything not in the range of (Q1 - 1.5 IQR) and (Q3 + 1.5 IQR) is an outlier, other rules for removing 'outliers' should be considered as well. For example, removing any values in the upper 1%. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n",
      "Age upon Outcome Days\n"
     ]
    }
   ],
   "source": [
    "for c in numerical_features_all:\n",
    "    print(c)\n",
    "    \n",
    "    # Drop values below Q1 - 1.5 IQR and beyond Q3 + 1.5 IQR\n",
    "    #Q1 = df[c].quantile(0.25)\n",
    "    #Q3 = df[c].quantile(0.75)\n",
    "    #IQR = Q3 - Q1\n",
    "    #print (Q1 - 1.5*IQR, Q3 + 1.5*IQR)\n",
    "    \n",
    "    #dropIndexes = df[df[c] > Q3 + 1.5*IQR].index\n",
    "    #df.drop(dropIndexes , inplace=True)\n",
    "    #dropIndexes = df[df[c] < Q1 - 1.5*IQR].index\n",
    "    #df.drop(dropIndexes , inplace=True)\n",
    "    \n",
    "    # Drop values beyond 90% of max()\n",
    "    dropIndexes = df[df[c] > df[c].max()*9/10].index\n",
    "    df.drop(dropIndexes , inplace=True)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n",
      "(-6.936, 693.5]     61425\n",
      "(693.5, 1387.0]     18400\n",
      "(1387.0, 2080.5]     5657\n",
      "(2080.5, 2774.0]     3471\n",
      "(2774.0, 3467.5]     2557\n",
      "(3467.5, 4161.0]     1962\n",
      "(4161.0, 4854.5]     1148\n",
      "(4854.5, 5548.0]      596\n",
      "(5548.0, 6241.5]      183\n",
      "(6241.5, 6935.0]       63\n",
      "Name: Age upon Intake Days, dtype: int64\n",
      "Age upon Outcome Days\n",
      "(-6.936, 693.5]     61208\n",
      "(693.5, 1387.0]     18490\n",
      "(1387.0, 2080.5]     5643\n",
      "(2080.5, 2774.0]     3465\n",
      "(2774.0, 3467.5]     2600\n",
      "(3467.5, 4161.0]     2004\n",
      "(4161.0, 4854.5]     1196\n",
      "(4854.5, 5548.0]      604\n",
      "(5548.0, 6241.5]      187\n",
      "(6241.5, 6935.0]       65\n",
      "Name: Age upon Outcome Days, dtype: int64\n"
     ]
    }
   ],
   "source": [
    "for c in numerical_features_all:\n",
    "    print(c)\n",
    "    print(df[c].value_counts(bins=10, sort=False))\n",
    "    plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's see the histograms again, with more bins for vizibility."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Outcome Days\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "for c in numerical_features_all:\n",
    "    print(c)\n",
    "    df[c].plot.hist(bins=100)\n",
    "    plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 4. <a name=\"4\">Scatter Plots and Correlation</a>\n",
    "(<a href=\"#0\">Go to top</a>)\n",
    "\n",
    "### Scatter plot\n",
    "Scatter plots are simple 2D plots of two numerical variables that can be used to examine the relationship between two variables. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1152x1152 with 4 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%matplotlib inline\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "fig, axes = plt.subplots(len(numerical_features_all), len(numerical_features_all), figsize=(16, 16), sharex=False, sharey=False)\n",
    "for i in range(0,len(numerical_features_all)):\n",
    "    for j in range(0,len(numerical_features_all)):\n",
    "        axes[i,j].scatter(x = df[numerical_features_all[i]], y = df[numerical_features_all[j]])\n",
    "fig.tight_layout()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Scatterplot with Identification\n",
    "\n",
    "We can also add the target values, 0 or 1, to our scatter plot."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import seaborn as sns\n",
    "\n",
    "X1 = df[[numerical_features_all[0], numerical_features_all[1]]][df[model_target] == 0]\n",
    "X2 = df[[numerical_features_all[0], numerical_features_all[1]]][df[model_target] == 1]\n",
    "\n",
    "plt.scatter(X1.iloc[:,0], \n",
    "            X1.iloc[:,1], \n",
    "            s=50, \n",
    "            c='blue', \n",
    "            marker='o', \n",
    "            label='0')\n",
    "\n",
    "plt.scatter(X2.iloc[:,0], \n",
    "            X2.iloc[:,1], \n",
    "            s=50, \n",
    "            c='red', \n",
    "            marker='v', \n",
    "            label='1')\n",
    "\n",
    "plt.xlabel(numerical_features_all[0])\n",
    "plt.ylabel(numerical_features_all[1])\n",
    "plt.legend()\n",
    "plt.grid()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Scatterplots with identification, can sometimes help identify whether or not we can get good separation between the data points, based on these two numerical features alone. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Correlation Matrix Heatmat\n",
    "We plot the correlation matrix. Correlation scores are calculated for numerical fields. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<style  type=\"text/css\" >\n",
       "    #T_5bdaa076_c321_11ea_b410_39fb856da49drow0_col0 {\n",
       "            background-color:  #d9d9d9;\n",
       "            color:  #000000;\n",
       "        }    #T_5bdaa076_c321_11ea_b410_39fb856da49drow0_col1 {\n",
       "            background-color:  #3182bd;\n",
       "            color:  #000000;\n",
       "        }    #T_5bdaa076_c321_11ea_b410_39fb856da49drow1_col0 {\n",
       "            background-color:  #3182bd;\n",
       "            color:  #000000;\n",
       "        }    #T_5bdaa076_c321_11ea_b410_39fb856da49drow1_col1 {\n",
       "            background-color:  #d9d9d9;\n",
       "            color:  #000000;\n",
       "        }</style><table id=\"T_5bdaa076_c321_11ea_b410_39fb856da49d\" ><thead>    <tr>        <th class=\"blank level0\" ></th>        <th class=\"col_heading level0 col0\" >Age upon Intake Days</th>        <th class=\"col_heading level0 col1\" >Age upon Outcome Days</th>    </tr></thead><tbody>\n",
       "                <tr>\n",
       "                        <th id=\"T_5bdaa076_c321_11ea_b410_39fb856da49dlevel0_row0\" class=\"row_heading level0 row0\" >Age upon Intake Days</th>\n",
       "                        <td id=\"T_5bdaa076_c321_11ea_b410_39fb856da49drow0_col0\" class=\"data row0 col0\" >1</td>\n",
       "                        <td id=\"T_5bdaa076_c321_11ea_b410_39fb856da49drow0_col1\" class=\"data row0 col1\" >0.998839</td>\n",
       "            </tr>\n",
       "            <tr>\n",
       "                        <th id=\"T_5bdaa076_c321_11ea_b410_39fb856da49dlevel0_row1\" class=\"row_heading level0 row1\" >Age upon Outcome Days</th>\n",
       "                        <td id=\"T_5bdaa076_c321_11ea_b410_39fb856da49drow1_col0\" class=\"data row1 col0\" >0.998839</td>\n",
       "                        <td id=\"T_5bdaa076_c321_11ea_b410_39fb856da49drow1_col1\" class=\"data row1 col1\" >1</td>\n",
       "            </tr>\n",
       "    </tbody></table>"
      ],
      "text/plain": [
       "<pandas.io.formats.style.Styler at 0x7f87a57f4160>"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cols=[numerical_features_all[0], numerical_features_all[1]]\n",
    "#print(df[cols].corr())\n",
    "df[cols].corr().style.background_gradient(cmap='tab20c')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Similar to scatterplots, but now the correlation matrix values can more clearly pinpoint relationships between the numerical features. Correlation values of -1 means perfect negative correlation, 1 means perfect positive correlation, and 0 means there is no relationship between the two numerical features."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### A fancy example using Seaborn"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x7f87a31fbd68>"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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zJkztWbNGj333HO666673FkTAACAVyiy0Tpz5oyWL1+ugIAAd9YDAADgNYpstKZNm+bOOgAAALwOi64AAABsQqMFAABgExotAAAAm9BoAQAA2MRhGIbh6SIAAAC8kdMNS6125kKmJTnBFf2V8cVmS7ICOtyltDUbTecERXWTJF069ZvprHIhf5AkZXz+lemsgE53S5KST541nVW/elUt3b7LdI4kxbVupnP/W2U6p/J990iSpVmZe/aZzvJvHCFJeuq9Faaz/jk4RjnHTpjOkaTyYTW08+BRS7Jahtey5OchrnUzSdK/PvncdNaT93aSJKWv/9R0VmCPLpKktNXrTWcF9e6h02+/ZzpHkqo9NljZvx42neNXt44kKfXDpaazqtwfJ0nKOWH+2rjla4RKktLT001nBQYG6vDwsaZzJKnO3FlK/ciay9FV+WOsNu05YDqnY+NbLKgGdmPqEAAAwCY0WgAAADah0QIAALAJjRYAAIBNaLQAAABsUuJG68KFC1bWAQAA4HWcNlrt27fX2rVrr/m1gQMH2lIQAACAt7juiNaMGTP0j3/8Q7/f15R9TgEAAJxz2miFhIQoISFBu3fv1tChQ3X27P/b9NLhcNheHAAAQGl23RGtqlWrat68ebr11lvVr18//fjjj5IY0QIAALieYl2Cx8fHR3/961912223acSIEXryyScZ0QIAALgOp43W70etevXqpQYNGig+Pl6HD5u/1hYAAIA3c9pojR1b+GKc9evX15IlS/TBBx/YVhQAAIA3cLpGq2vXrtc8XqlSJY0YMcKWggAAALwFO8MDAADYhEYLAADAJjRaAAAANnEYbIgFAABgC0a0AAAAbEKjBQAAYBMaLQAAAJvQaAEAANiERgsAAMAmNFoAAAA2odECAACwCY0WAACATWi0AAAAbHLDNFrnzp1Ts2bN9OKLL5rK6dq1q6KiohQTE6OoqChNnjxZOTk5Jc7LycnRq6++ql69eunee+9V7969NX36dJcyr9R03333qUePHho1apS++eabEtd09fd45XbkyBFLsqZNm1ainJycHL322mv571NMTIzGjh2r/fv3l6imffv2FTgWFxenrVu3lqi2ojI9mXV1xsWLFzVs2DBNmjRJubm5Hqunffv2BV4/ISFBjRo10vvvv1+ivOjoaOXl5ZmuMzs7W9OnT1f37t0VFRWl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\n",
      "text/plain": [
       "<Figure size 792x648 with 2 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "from string import ascii_letters\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "import seaborn as sns\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "sns.set(style=\"white\")\n",
    "\n",
    "# Generate a large random dataset\n",
    "rs = np.random.RandomState(33)\n",
    "d = pd.DataFrame(data=rs.normal(size=(100, 26)),\n",
    "                 columns=list(ascii_letters[26:]))\n",
    "\n",
    "# Compute the correlation matrix\n",
    "corr = d.corr()\n",
    "\n",
    "# Generate a mask for the upper triangle\n",
    "mask = np.triu(np.ones_like(corr, dtype=np.bool))\n",
    "\n",
    "# Set up the matplotlib figure\n",
    "f, ax = plt.subplots(figsize=(11, 9))\n",
    "\n",
    "# Generate a custom diverging colormap\n",
    "cmap = sns.diverging_palette(220, 10, as_cmap=True)\n",
    "\n",
    "# Draw the heatmap with the mask and correct aspect ratio\n",
    "sns.heatmap(corr, mask=mask, cmap=cmap, vmax=.3, center=0,\n",
    "            square=True, linewidths=.5, cbar_kws={\"shrink\": .5})"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Also, more exploratory data analysis might reveal other important hidden atributes and/or relationships of the model features considered. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 5. <a name=\"5\">Handling Missing Values</a>\n",
    "(<a href=\"#0\">Go to top</a>)\n",
    "\n",
    "  * <a href=\"#51\">Drop columns with missing values</a>\n",
    "  * <a href=\"#52\">Drop rows with missing values</a>\n",
    "  * <a href=\"#53\"> Impute (fill-in) missing values with .fillna()</a>\n",
    "  * <a href=\"#54\"> Impute (fill-in) missing values with sklearn's SimpleImputer</a>\n",
    "\n",
    "Let's first check the number of missing (nan) values for each column."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Pet ID                       0\n",
       "Outcome Type                 0\n",
       "Sex upon Outcome             1\n",
       "Name                     36343\n",
       "Found Location               0\n",
       "Intake Type                  0\n",
       "Intake Condition             0\n",
       "Pet Type                     0\n",
       "Sex upon Intake              1\n",
       "Breed                        0\n",
       "Color                        0\n",
       "Age upon Intake Days         0\n",
       "Age upon Outcome Days        0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.isna().sum()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's explore a few options dealing with missing values, when there are values missing on many features, both numerical and categorical types. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### <a name=\"51\">Drop columns with missing values</a>\n",
    "(<a href=\"#5\">Go to Handling Missing Values</a>)\n",
    "\n",
    "We can drop some feautures/columns if we think there is significant amount of missing data in those features. Here we \n",
    "are dropping features having more than 20% missing values.\n",
    "\n",
    "__Hint:__ You can also use __inplace=True__ parameter to drop features inplace without assignment.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Pet ID                   0.000000\n",
      "Outcome Type             0.000000\n",
      "Sex upon Outcome         0.000010\n",
      "Name                     0.380706\n",
      "Found Location           0.000000\n",
      "Intake Type              0.000000\n",
      "Intake Condition         0.000000\n",
      "Pet Type                 0.000000\n",
      "Sex upon Intake          0.000010\n",
      "Breed                    0.000000\n",
      "Color                    0.000000\n",
      "Age upon Intake Days     0.000000\n",
      "Age upon Outcome Days    0.000000\n",
      "dtype: float64\n",
      "Index(['Name'], dtype='object')\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Pet ID</th>\n",
       "      <th>Outcome Type</th>\n",
       "      <th>Sex upon Outcome</th>\n",
       "      <th>Found Location</th>\n",
       "      <th>Intake Type</th>\n",
       "      <th>Intake Condition</th>\n",
       "      <th>Pet Type</th>\n",
       "      <th>Sex upon Intake</th>\n",
       "      <th>Breed</th>\n",
       "      <th>Color</th>\n",
       "      <th>Age upon Intake Days</th>\n",
       "      <th>Age upon Outcome Days</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>A794011</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>Austin (TX)</td>\n",
       "      <td>Owner Surrender</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Brown Tabby/White</td>\n",
       "      <td>730</td>\n",
       "      <td>730</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>A776359</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>7201 Levander Loop in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Dog</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Chihuahua Shorthair Mix</td>\n",
       "      <td>White/Brown</td>\n",
       "      <td>365</td>\n",
       "      <td>365</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>A674754</td>\n",
       "      <td>0.0</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>12034 Research in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Nursing</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Orange Tabby</td>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>A689724</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>2300 Waterway Bnd in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Normal</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>Black</td>\n",
       "      <td>60</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>A680969</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Neutered Male</td>\n",
       "      <td>4701 Staggerbrush Rd in Austin (TX)</td>\n",
       "      <td>Stray</td>\n",
       "      <td>Nursing</td>\n",
       "      <td>Cat</td>\n",
       "      <td>Intact Male</td>\n",
       "      <td>Domestic Shorthair Mix</td>\n",
       "      <td>White/Orange Tabby</td>\n",
       "      <td>7</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Pet ID  Outcome Type Sex upon Outcome  \\\n",
       "0  A794011           1.0    Neutered Male   \n",
       "1  A776359           1.0    Neutered Male   \n",
       "2  A674754           0.0      Intact Male   \n",
       "3  A689724           1.0    Neutered Male   \n",
       "4  A680969           1.0    Neutered Male   \n",
       "\n",
       "                        Found Location      Intake Type Intake Condition  \\\n",
       "0                          Austin (TX)  Owner Surrender           Normal   \n",
       "1    7201 Levander Loop in Austin (TX)            Stray           Normal   \n",
       "2        12034 Research in Austin (TX)            Stray          Nursing   \n",
       "3     2300 Waterway Bnd in Austin (TX)            Stray           Normal   \n",
       "4  4701 Staggerbrush Rd in Austin (TX)            Stray          Nursing   \n",
       "\n",
       "  Pet Type Sex upon Intake                    Breed               Color  \\\n",
       "0      Cat   Neutered Male   Domestic Shorthair Mix   Brown Tabby/White   \n",
       "1      Dog     Intact Male  Chihuahua Shorthair Mix         White/Brown   \n",
       "2      Cat     Intact Male   Domestic Shorthair Mix        Orange Tabby   \n",
       "3      Cat     Intact Male   Domestic Shorthair Mix               Black   \n",
       "4      Cat     Intact Male   Domestic Shorthair Mix  White/Orange Tabby   \n",
       "\n",
       "   Age upon Intake Days  Age upon Outcome Days  \n",
       "0                   730                    730  \n",
       "1                   365                    365  \n",
       "2                     6                      6  \n",
       "3                    60                     60  \n",
       "4                     7                     60  "
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "threshold = 2/10\n",
    "print((df.isna().sum()/len(df.index)))\n",
    "columns_to_drop = df.loc[:,list(((df.isna().sum()/len(df.index))>=threshold))].columns    \n",
    "print(columns_to_drop)\n",
    "\n",
    "df_columns_dropped = df.drop(columns_to_drop, axis = 1)  \n",
    "df_columns_dropped.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Pet ID                   0\n",
       "Outcome Type             0\n",
       "Sex upon Outcome         1\n",
       "Found Location           0\n",
       "Intake Type              0\n",
       "Intake Condition         0\n",
       "Pet Type                 0\n",
       "Sex upon Intake          1\n",
       "Breed                    0\n",
       "Color                    0\n",
       "Age upon Intake Days     0\n",
       "Age upon Outcome Days    0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_columns_dropped.isna().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(95462, 12)"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_columns_dropped.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Note the reduced size of the dataset features. This can sometimes lead to underfitting models -- not having enough features to build a good model able to capture the pattern in the dataset, especially when dropping features that are essential to the task at hand. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### <a name=\"52\">Drop rows with missing values</a>\n",
    "(<a href=\"#5\">Go to Handling Missing Values</a>)\n",
    "\n",
    "Here, we simply drop rows that have at least one missing value. There are other drop options to explore, depending on specific problems."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_missing_dropped = df.dropna()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's check the missing values below."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Pet ID                   0\n",
       "Outcome Type             0\n",
       "Sex upon Outcome         0\n",
       "Name                     0\n",
       "Found Location           0\n",
       "Intake Type              0\n",
       "Intake Condition         0\n",
       "Pet Type                 0\n",
       "Sex upon Intake          0\n",
       "Breed                    0\n",
       "Color                    0\n",
       "Age upon Intake Days     0\n",
       "Age upon Outcome Days    0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_missing_dropped.isna().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(59118, 13)"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_missing_dropped.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This approach can dramatically reduce the number of data samples. This can sometimes lead to overfitting models -- especially when the number of features is greater or comparable to the number of data samples. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### <a name=\"53\">Impute (fill-in) missing values with .fillna()</a>\n",
    "(<a href=\"#5\">Go to Handling Missing Values</a>)\n",
    "\n",
    "Rather than dropping rows (data samples) and/or columns (features), another strategy to deal with missing values would be to actually complete the missing values with new values: imputation of missing values.\n",
    "\n",
    "__Imputing Numerical Values:__ The easiest way to impute numerical values is to get the __average (mean) value__ for the corresponding column and use that as the new value for each missing record in that column. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days     0\n",
      "Age upon Outcome Days    0\n",
      "dtype: int64\n",
      "Age upon Intake Days     0\n",
      "Age upon Outcome Days    0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute numerical features by using the mean per feature to replace the nans\n",
    "\n",
    "# Assign our df to a new df \n",
    "df_imputed = df.copy()\n",
    "print(df_imputed[numerical_features_all].isna().sum())\n",
    "\n",
    "# Impute our two numerical features with the means. \n",
    "df_imputed[numerical_features_all] = df_imputed[numerical_features_all].fillna(df_imputed[numerical_features_all].mean())\n",
    "\n",
    "print(df_imputed[numerical_features_all].isna().sum())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "__Imputing Categorical Values:__ We can impute categorical values by getting the most common (mode) value for the corresponding column and use that as the new value for each missing record in that column. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Pet ID                  0\n",
      "Sex upon Outcome        1\n",
      "Name                36343\n",
      "Found Location          0\n",
      "Intake Type             0\n",
      "Intake Condition        0\n",
      "Pet Type                0\n",
      "Sex upon Intake         1\n",
      "Breed                   0\n",
      "Color                   0\n",
      "dtype: int64\n",
      "Pet ID 0        A047759\n",
      "1        A134067\n",
      "2        A141142\n",
      "3        A163459\n",
      "4        A165752\n",
      "          ...   \n",
      "95457    A819862\n",
      "95458    A819864\n",
      "95459    A819865\n",
      "95460    A819895\n",
      "95461    A819908\n",
      "Length: 95462, dtype: object\n",
      "Sex upon Outcome 0    Neutered Male\n",
      "dtype: object\n",
      "Name 0    Bella\n",
      "dtype: object\n",
      "Found Location 0    Austin (TX)\n",
      "dtype: object\n",
      "Intake Type 0    Stray\n",
      "dtype: object\n",
      "Intake Condition 0    Normal\n",
      "dtype: object\n",
      "Pet Type 0    Dog\n",
      "dtype: object\n",
      "Sex upon Intake 0    Intact Male\n",
      "dtype: object\n",
      "Breed 0    Domestic Shorthair Mix\n",
      "dtype: object\n",
      "Color 0    Black/White\n",
      "dtype: object\n",
      "Pet ID              0\n",
      "Sex upon Outcome    0\n",
      "Name                0\n",
      "Found Location      0\n",
      "Intake Type         0\n",
      "Intake Condition    0\n",
      "Pet Type            0\n",
      "Sex upon Intake     0\n",
      "Breed               0\n",
      "Color               0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute categorical features by using the mode per feature to replace the nans\n",
    "\n",
    "# Assign our df to a new df \n",
    "df_imputed_c = df.copy()\n",
    "print(df_imputed_c[categorical_features_all].isna().sum())\n",
    "\n",
    "for c in categorical_features_all:\n",
    "    # Find the mode per each feature\n",
    "    mode_impute = df_imputed_c[c].mode()\n",
    "    print(c, mode_impute)\n",
    "\n",
    "    # Impute our categorical features with the mode\n",
    "    # \"inplace=True\" parameter replaces missing values in place (no need for left handside assignment)\n",
    "    df_imputed_c[c].fillna(False, inplace=True)\n",
    "\n",
    "print(df_imputed_c[categorical_features_all].isna().sum())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We can also create a new category, such as \"Missing\", for alll or elected categorical features."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Pet ID                  0\n",
      "Sex upon Outcome        1\n",
      "Name                36343\n",
      "Found Location          0\n",
      "Intake Type             0\n",
      "Intake Condition        0\n",
      "Pet Type                0\n",
      "Sex upon Intake         1\n",
      "Breed                   0\n",
      "Color                   0\n",
      "dtype: int64\n",
      "Pet ID              0\n",
      "Sex upon Outcome    0\n",
      "Name                0\n",
      "Found Location      0\n",
      "Intake Type         0\n",
      "Intake Condition    0\n",
      "Pet Type            0\n",
      "Sex upon Intake     0\n",
      "Breed               0\n",
      "Color               0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute categorical features by using a placeholder value\n",
    "\n",
    "# Assign our df to a new df \n",
    "df_imputed = df.copy()\n",
    "print(df_imputed[categorical_features_all].isna().sum())\n",
    "\n",
    "# Impute our categorical features with a new category named \"Missing\". \n",
    "df_imputed[categorical_features_all]= df_imputed[categorical_features_all].fillna(\"Missing\")\n",
    "\n",
    "print(df_imputed[categorical_features_all].isna().sum())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### <a name=\"54\">Impute (fill-in) missing values with sklearn's __SimpleImputer__</a>\n",
    "(<a href=\"#5\">Go to Handling Missing Values</a>)\n",
    "\n",
    "A more elegant way to implement imputation is using sklearn's __SimpleImputer__, a class implementing .fit() and .transform() methods.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Age upon Intake Days     0\n",
      "Age upon Outcome Days    0\n",
      "dtype: int64\n",
      "Age upon Intake Days     0\n",
      "Age upon Outcome Days    0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute numerical columns by using the mean per column to replace the nans\n",
    "\n",
    "from sklearn.impute import SimpleImputer\n",
    "\n",
    "# Assign our df to a new df\n",
    "df_sklearn_imputed = df.copy()\n",
    "print(df_sklearn_imputed[numerical_features_all].isna().sum())\n",
    "\n",
    "imputer = SimpleImputer(strategy='mean')\n",
    "df_sklearn_imputed[numerical_features_all] = imputer.fit_transform(df_sklearn_imputed[numerical_features_all])\n",
    "\n",
    "print(df_sklearn_imputed[numerical_features_all].isna().sum())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Index(['Name', 'Sex upon Intake'], dtype='object')\n",
      "Name               36343\n",
      "Sex upon Intake        1\n",
      "dtype: int64\n",
      "Name               0\n",
      "Sex upon Intake    0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute categorical columns by using the mode per column to replace the nans\n",
    "\n",
    "# Pick some categorical features you desire to impute with this approach\n",
    "categoricals_missing_values = df[categorical_features_all].loc[:,list(((df[categorical_features_all].isna().sum()/len(df.index)) > 0.0))].columns    \n",
    "columns_to_impute = categoricals_missing_values[1:3]\n",
    "print(columns_to_impute)\n",
    "\n",
    "from sklearn.impute import SimpleImputer\n",
    "\n",
    "# Assign our df to a new df\n",
    "df_sklearn_imputer = df.copy()\n",
    "print(df_sklearn_imputer[columns_to_impute].isna().sum())\n",
    "\n",
    "imputer = SimpleImputer(strategy='most_frequent')\n",
    "df_sklearn_imputer[columns_to_impute] = imputer.fit_transform(df_sklearn_imputer[columns_to_impute])\n",
    "\n",
    "print(df_sklearn_imputer[columns_to_impute].isna().sum())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Index(['Name', 'Sex upon Intake'], dtype='object')\n",
      "Name               36343\n",
      "Sex upon Intake        1\n",
      "dtype: int64\n",
      "Name               0\n",
      "Sex upon Intake    0\n",
      "dtype: int64\n"
     ]
    }
   ],
   "source": [
    "# Impute categorical columns by using a placeholder \"Missing\"\n",
    "\n",
    "# Pick some categorical features you desire to impute with this approach\n",
    "categoricals_missing_values = df[categorical_features_all].loc[:,list(((df[categorical_features_all].isna().sum()/len(df.index)) > 0.0))].columns    \n",
    "columns_to_impute = categoricals_missing_values[1:3]\n",
    "print(columns_to_impute)\n",
    "\n",
    "from sklearn.impute import SimpleImputer\n",
    "\n",
    "# Assign our df to a new df\n",
    "df_sklearn_imputer = df.copy()\n",
    "print(df_sklearn_imputer[columns_to_impute].isna().sum())\n",
    "\n",
    "imputer = SimpleImputer(strategy='constant', fill_value = \"Missing\")\n",
    "df_sklearn_imputer[columns_to_impute] = imputer.fit_transform(df_sklearn_imputer[columns_to_impute])\n",
    "\n",
    "print(df_sklearn_imputer[columns_to_impute].isna().sum())"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "conda_mxnet_p36",
   "language": "python",
   "name": "conda_mxnet_p36"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.10"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}

