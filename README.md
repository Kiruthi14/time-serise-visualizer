import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def read_data():
    # Read the dataset and set the date column as index
    df = pd.read_csv("fcc-forum-pageviews.csv", index_col="date", parse_dates=["date"])
    return df

def clean_data(df):
    # Filter out the top 2.5% and bottom 2.5% of the page views
    lower_percentile = df['page_views'].quantile(0.025)
    upper_percentile = df['page_views'].quantile(0.975)
    df = df[(df['page_views'] >= lower_percentile) & (df['page_views'] <= upper_percentile)]
    return df

def draw_line_plot(df):
    # Draw a line plot
    plt.figure(figsize=(12, 6))
    plt.plot(df.index, df['page_views'], color='b', lw=1)
    plt.title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
    plt.xlabel('Date')
    plt.ylabel('Page Views')
    plt.tight_layout()
    plt.savefig('line_plot.png')
    plt.show()

def draw_bar_plot(df):
    # Group data by year and month, then calculate the average page views
    df['year'] = df.index.year
    df['month'] = df.index.month
    monthly_avg = df.groupby(['year', 'month']).mean()['page_views'].unstack()

    # Draw a bar plot
    monthly_avg.plot(kind='bar', figsize=(12, 6))
    plt.title('Average Daily Page Views per Month')
    plt.xlabel('Years')
    plt.ylabel('Average Page Views')
    plt.legend(title='Months', labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
    plt.tight_layout()
    plt.savefig('bar_plot.png')
    plt.show()

def draw_box_plot(df):
    # Extract year and month information
    df['year'] = df.index.year
    df['month'] = df.index.month

    # Year-wise box plot (Trend)
    plt.figure(figsize=(12, 6))
    sns.boxplot(x='year', y='page_views', data=df)
    plt.title('Year-wise Box Plot (Trend)')
    plt.xlabel('Year')
    plt.ylabel('Page Views')
    plt.tight_layout()
    plt.savefig('year_box_plot.png')
    plt.show()

    # Month-wise box plot (Seasonality)
    plt.figure(figsize=(12, 6))
    sns.boxplot(x='month', y='page_views', data=df)
    plt.title('Month-wise Box Plot (Seasonality)')
    plt.xlabel('Month')
    plt.ylabel('Page Views')
    plt.tight_layout()
    plt.savefig('month_box_plot.png')
    plt.show()

def main():
    # Load the data
    df = read_data()

    # Clean the data
    df = clean_data(df)

    # Draw the line plot
    draw_line_plot(df)

    # Draw the bar plot
    draw_bar_plot(df)

    # Draw the box plots
    draw_box_plot(df)

if __name__ == "__main__":
    main()
