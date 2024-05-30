# purpose

- GitHub's GraphQL API allows fetching up to 100 records at a time
- this limitation can pose challenges for users needing more extensive data, such as:
  - backing up all issues from a repository
  - analyzing issues, for example, to:
    - calculate the average duration an issue remains open
    - count issues by specific labels
- if your repository contains more than 100 issues, you need to:
  - first, fetch 100 issues at a time
  - then store them in a file
  - then, use the [PageInfo](https://docs.github.com/en/graphql/reference/objects#pageinfo) object from the last fetched record to fetch the next 100 issues in the subsequent request
  - keep doing this till the desired number of records are fetched
- that is a lot of work
- the jupyter notebooks do the hard work for you
- they automatically paginate through GitHub issues, and store new data to an existing file in each iteration
- the final product you get is a single CSV file that can be used for further analysis and visualizations [like these ](/400-visualize_data/400-visualize_data.ipynb)

## folder structure

- **320-fetch_first_100_closed_issues**

  - [link](./320-fetch_first_100_closed_issues/320-fetch_first_100_closed_issues.ipynb)
  - fetches the FIRST 100 closed issues from your repository
  - you do not have to do anything in this file
  - it will be executed automatically by another file

- **340-fetch_next_100_closed_issues**

  - [link](./340-fetch_next_100_closed_issues/340-fetch_next_100_closed_issues.ipynb)
  - fetches the NEXT 100 closed issues from your repository, based on where the previous run left off
  - you do not have to do anything in this file
  - it will be executed automatically by another file

- **360-paginate_data**
  - [link](./360-paginate_data/360-paginate_data.ipynb)
  - this is the file that executes the previous 2 files
  - it paginates through all GitHub closed issues, fetching 100 records at a time, and appends the new data to an existing CSV file

# logic

- **pagination**
  - [360-paginate_data](./360-paginate_data/360-paginate_data.ipynb) has the code that fetches issues in batches of 100, utilizing a [counter](./360-paginate_data/counter.json) to track progress
  - the counter file ensures that your cursor for fetching records is placed at the right location
- **aggregation**
  - each fetched batch of issues is appended to an [aggregated data file](./360-paginate_data/366-aggregated_data.csv) for further analysis
  - the [visualizations](./../400-visualize_data/400-visualize_data.ipynb) are built on top of this aggregated data file
