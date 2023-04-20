import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt


def avg_stsf_score(file_path):
    # Read the data 
    data = pd.read_excel(file_path)

    avg_satisfaction_scores = data.groupby('Center')['NPS'].agg(['mean', 'std'])

    # Create a line plot with the mean score as a red line and the standard deviation as a shaded area
    sns.lineplot(data=avg_satisfaction_scores['mean'], color='red')
    plt.fill_between(avg_satisfaction_scores.index,
                     avg_satisfaction_scores['mean'] - avg_satisfaction_scores['std'],
                     avg_satisfaction_scores['mean'] + avg_satisfaction_scores['std'],
                     alpha=0.2)

    plt.xlabel('Center')
    plt.ylabel('Average NPS score')
    plt.title('Average NPS score and Standard Deviation by Center')
    plt.show()


avg_stsf_score("Case_study_data.xlsx")


def calc_nps(data_file):
    data = pd.read_excel(data_file)

    # Convert the Month column to a datetime format
    data['Month'] = pd.to_datetime(data['Month'], format='%m/%Y')

    data.set_index('Month', inplace=True)

    promoters = data[data['NPS'] >= 9].groupby(['Center', pd.Grouper(freq='Y')])['NPS'].count()
    detractors = data[data['NPS'] <= 6].groupby(['Center', pd.Grouper(freq='Y')])['NPS'].count()
    nps = (promoters - detractors) / data.groupby(['Center', pd.Grouper(freq='Y')])['NPS'].count() * 100

    nps_pivot = nps.unstack(level=0)

    # Create a bar chart with one bar per center per year
    fig, ax = plt.subplots(figsize=(12, 8))
    x = range(len(nps_pivot.index))
    width = 1.0 / (len(nps_pivot.columns) + 1)
    for i, center in enumerate(nps_pivot.columns):
        ax.bar([xi + i * width for xi in x], nps_pivot[center], width=0.01, label=None, tick_label=None, color='blue')

    ax.set_xticks(x)
    ax.set_xticklabels([year.year for year in nps_pivot.index])

    ax.set_xlabel('Year')
    ax.set_ylabel('NPS')
    ax.set_title('NPS per Center per Year')

    ax.legend(title='Center', loc='upper left', labels=None)

    plt.show()


calc_nps("Case_study_data.xlsx")
