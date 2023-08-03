# **Lab Manual**

The Lab provides a multi-purpose environment for data exploration, AI/ML modeling, and application development, based on [Jupyter Lab](https://github.com/jupyterlab/jupyterlab).

To use **ThanoSQL** in this environment, you must first load the **ThanoSQL Magic**.

!!! tip ""
    You can press the run button at the top, or you can run it with either the **Ctrl + Enter** or **Shift + Enter** shortcuts.

## **1. Loading ThanoSQL Magic**

Load the ThanoSQL Magic extension program with the following command.

```sql
%load_ext thanosql
```

## **2. Setting API_TOKEN**

Set the API_TOKEN obtained through **Workspace Settings page - Developer - API Token - Copy button** as follows.

```sql
%thanosql API_TOKEN=<Issued_API_TOKEN>
```

ex)

```sql
%thanosql API_TOKEN=eyGVFDdfafddvczs
```

!!! tip "With the generated API token, you can use all of the ThanoSQL`s REST APIs"
    For more information about using the ThanoSQL's REST API, see [__ThanoSQL REST API Reference__](/en/how-to_guides/reference/#thanosql-rest-api-reference)

## **3. Check the list of the ThanoSQL models and datasets using the LIST query syntax**

All preparations to use **ThanoSQL** are complete.

To view a list of prebuilt the ThanoSQL models, run the ThanoSQL statement below.

```sql
%%thanosql
LIST THANOSQL MODEL
```

[![IMAGE](/img/getting_started/img8.png)](/img/getting_started/img8.png)

To see the list of datasets used by the tutorials, run the statement below.

```sql
%%thanosql
LIST THANOSQL DATASET
```

[![IMAGE](/img/getting_started/img9.png)](/img/getting_started/img9.png)


!!! notice "How to write queries in the ThanoSQL Workspace Lab" 
    There are two ways to write queries in ThanoSQL: line-based and cell-based.

    - Line-based: After entering `%thanosql`, continue writing the query on the same line. The query result is returned as a DataFrame and can be stored in a variable for further programming, similar to a Python variable.
    
    ```
    df = %thanosql SELECT id, name FROM user
    ```

    - Cell-based: After entering `%%thanosql`, write the query freely on multiple lines. This provides a user experience similar to using other DBMSs to query tables or execute ThanoSQL extension syntax.

    ```
    %%thanosql
    SELECT
        id,
        name
    FROM user
    ```


## __4. Get Tutorial__

You can check out the available tutorials from the [tutorials in the ThanoSQL Technical Documentation](/en/tutorials/algorithm_list/). 
Running the statement below allows you to clone all the ThanoSQL's tutorial into your workspace.

```sql
!git clone https://github.com/smartmind-team/thanosql-tutorial.git
```

If you want to import only certain tutorials into your workspace, use the wget method as shown below.

!!!note "___Github URL of the ThanoSQL tutorials___"

```python
!wget [Tutorials' Github URL]
# Example 
## wget https://raw.githubusercontent.com/smartmind-team/thanosql-tutorial/main/tutorial_en/thanosql_search/search_image_by_keyword.ipynb
```
