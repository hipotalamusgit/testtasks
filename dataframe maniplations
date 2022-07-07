# HOW TO ADD A COLUMN IN DATAFRAME BASED ON CONDITION THAT CERTAIN SUBSTRING CAN BE EXTRACTED FROM ELEMENTS OF THAT COLUMN
#RESULTING COLUMN CONTAINS SUBSTRING IF CONDINTION IS TRUE AND ORIGINAL DATA IF FALSE
import numpy as np
import pandas as pd

data = {'event_name': ['emailJai', 'Prinemailci', 'Gaurav', 'Anuj'],
        'Height': [5.1, 6.2, 5.1, 5.2],
        'Qualification': ['Msc', 'MA', 'Msc', 'Msc']}
df = pd.DataFrame(data)
print(df)
df['email_event'] = df['event_name'].str.extract(r'(email)')[0]
df['new_event'] = np.where(df['email_event']== "email", df['email_event'], df['event_name'])
df.drop('email_event', axis=1, inplace=True)
print(df)
