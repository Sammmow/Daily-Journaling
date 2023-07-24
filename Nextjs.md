
<br>
**June 2nd,2023**


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