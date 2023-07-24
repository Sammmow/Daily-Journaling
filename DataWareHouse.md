Data warehouse: 
27th June,2023

ETL tools:   
Which one to use? :- tools such that it automates ETL processes.  
There are tools such as Talend, Informatica Power Center, Apache Airflow, Microsoft SQL Server Integration Service.

28th June,2023

Using Data Ware House

1. Understand data source: 
Data from : Boss, KYC and tms.
first step is to identify format,structures and connection requirement.

* Connectors: ETLs offer method to extract data from a source and place it in a destination. In this case, the destination is Data Warehouse.

2. Extract data using connectors.

3. Data Transformation: Data cleaning, data filtering, Data integration: data have varying sources ,data have different formats.
   It involves merging or integrating data from multiple sources into a unified format that fits target data warehouse schema.

   Suppose,there are two data files. i.e. one file has a client code and one has a column called phone number. And suppose, another has a client code and a column 
   called ph.no. So, here the columns names are conflicting. 
  How to resolve it?
-> First, specify column names and data types as they exist in the respective files.
-> Define target definition. Define target definition that represents desired schema with standarized column names.
-> Metadata management. 
-> Store column mapping data in metadata repo including original names and corresponding standardized names.
-> Define strategy that retrieves column map from repo and use it to change the column name to standardized name.


<br/>
Personal journey:
I started researching on data warehouses and data lakes.I came to find out that data warehouses are for structured data,etc whereas in data lakes, we can put in like 
unstructured data as well. 
On June 28th,2023 I researched on various ETL tools and well researched on Informatica Power Center.
Informatica Power center is an enterprise level ETL tool. It supports db, Oracle ,Microsoft SQL,MySQL,flatfiles, etc.
**Roadblock** : The roadblock I had today was I did not know what to look for. I know I have to look ETL tools and see about what tools I can use to merge files having
conflicts. I started looking on the ETL connectors and found out that Informatica Power center supports various data files. 
But,I did not know where to look or what to search for. 

Solution: I asked Nishant dai what to do? Bujhena bhanera. He simplified the problem for me.There are two csv files and there are conflicting names in the column. 
So, just imagine there is temporary address and address in two csv files. When merging files, the conflict that comes up is address rakhne ki temporary address. Ani dubai 
ma data cha bhane kasari milaune teslai.
For eg, 2 lakh data cha re euta csv ma ani 1 lakh data cha re arko csv ma. What then?
I thought about it and asked chatgpt and found out that, there is something called MetaData Management that lets you create data mapping such that, data warehouse ma
chai address matra bascha. To do that, we create a dictionary such that the address and temp address from the two address files are mapped such that sab address hos ya
temp address hos, temsa address only(standarized format) bascha.

<br>
**June 2nd,2023**

<img src= "C:\downloads\\problem"> 

**Problem**: The problem I am facing today is the inputfield becomes empty only when the response comes. I need the inputfield cleared when the user enter submit. 

Solution1 - 
Using **debounce** func:
Debounce is a technique used in js to control how often a function is called. It ensures that a function is executed only after a certain amount of time has paased without the function being invoked again.

**UseCases** : Delay execution of function until a series of rapid-fire events have occured.

**Working**:





Solution2: - onSubmit make it such that after user hits enter or submit the query goes away from the placeholder.

First, I go to the handleSubmit() function.

const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    mutate(query);
--->setQuery('');

  };
I add the setQuery('') to handleSubmit.

Problem: When the user hits enter, the text in the input field evaporates but as I thought, the question in the display field also evaporates.This is because, the questionResponse = query.
And as i used the query everywhere, when i set query to null, it goes away.

Solution: Make another variable called question and assign it with query's value. Inplace of query in questionResponse, add the question instead. But that did not work. I do not know how to pass question variable to the mutate query useState.
Another solution is setting the questionResponse just after the mutatequery has been called.
This allows the query to be stored in questionResponse before the placeholder's query value can be set to null.

**OnSubmit Component**:
const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    mutate(query);
    setQuery('')
  };

  **useMutation Hook**:

const { isLoading, error, mutate } = useMutation(['post_question', query], (query: string) => 
  {
    setText([...text,
    {
      questionResponse:query
    }
    ])
    return(

      instance.post('/api/v1/chat', 
      { 'question': query }, 
      { headers: { "Authorization": `Bearer ${access_token}` } }
    ))
  }
  , {
    onSuccess: (response) => {
      console.log(query)
      const queAns: QueAns = {
        questionResponse: query,
        answerResponse: response?.data[0].answer,
      };

The logic behind this is to set the query before Success response. Because, on the handleSubmit component, I set the query to null so that the query does not submit after the user hits enter.

Before this, I tried to make a variable called question and give the variable the value of query.

 <TextField id="Query" name="query" variant="filled" multiline maxRows={2} value={query} onChange={handleChange} onKeyDown={handleKeyDown} sx={{ width: "60vw" }} placeholder={"Enter Text Here"} />

 It did not work because the "question" would show on the isLoading component but
 after the response is fetched from the api, the questionResponse would be null as query was set to setQuery('').  
 Hence, it would not fulfill my role.

 I also tried to make the isLoading component inside the data mapping.
 <Box display="flex" flexDirection="column" gap="15px">
              {text?.map((data, index) => (
                data.answerResponse && 
                <Box key={index}>
                  <Box display={"flex"} gap='65px'>{<Chip avatar={<Avatar></Avatar>} label="You" sx={{ mb: 1 }} />}{data.questionResponse}</Box>
                  <Box display={"flex"} gap={"10px"}>
                    <Chip label="NAASA Chatbot" sx={{ mr: 1 }} /> <Typography>{data.answerResponse}</Typography>
                  </Box>
                </Box>
              ))}
              {isLoading && (
                <>
                  <Box display={"flex"} gap="65px">{<Chip avatar={<Avatar></Avatar>} label="You" sx={{ mb: 1 }} />} {text[text?.length -1]?.questionResponse}</Box>
                  <Box display={"flex"} gap="10px"> 
                    <Chip label="NAASA Chatbot" sx={{ mr: 1 }} />  <CircularProgress size={20} />
                  </Box>
                </>
              )}
</Box>

Because I did not know how to write text[text?.length -1]?.questionResponse. 
text[text?.length -1]?.questionResponse gives the last index of the question. As I did not know about how to do this, I spent a significant time inorder to map the text array through the text?map((data,index)) but the problem with it was although I could get the questionResponse, the circularprogress would load on every submit.
i.e. circularProgress on every query I have asked till now would load. It load because the isLoading component was inside the {text?.map(data, index)}.
I had faced this problem before so, I resolved it easily. However, thinking up an alternative way took some time.

I also tried looking at problems similar to mine. I searched on one git repo : https://github.com/modal-labs/quillman/tree/main/src/fronten.
I read about the button components but I could not find a matching usecase and the repo button use was dissimilar to my use. I took some time to read it but was not able to understand most of it.

Now,I want to test my page but my token limit has been crossed and I cannot use the gpt-3.5-turbo api.

<br>

**Data Warehouse**:
I searched up Pentaho from the following link: https://help.hitachivantara.com/Documentation/Pentaho/8.3/Products/Learn_about_the_PDI_client#r_pentaho_data_integration_perspective_learn_about_the_pdi_client_spoon_pdi

I am reading on it. There is nothing written about connectors it provides.
It has some tools for locally extracting,transforming and loading.
Similarly, for bigger data, it uses spark engine. Ref here :https://help.hitachivantara.com/Documentation/Pentaho/8.3/Products/Adaptive_Execution_Layer
The pricing of products are not clearly listed or I have not been able to find them.

I looked at AWS Data Pipeline. The users can specify tasks to extract,transform.
It fea