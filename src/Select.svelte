<script>
    $: questionsanswers = [{
        id:1,
        question:'how are you',
        answer:''
    },{
        id:2,
        question:'how is school',
        answer:''
    },{
        id:3,
        question:'how is job',
        answer:''
    }]
    let selected;
    $: answer="";
    const handlesubmit = () =>{
        alert("selected are")
    }
 const handlechange =(question)=>{
    let questionsanswers_copy = questionsanswers.slice();
        console.log(questionsanswers_copy);
        const currentQs = questionsanswers_copy.find((q) => q.question === question );
        if(currentQs.answer != '')
        {
            answer=currentQs.answer
        }
        else
        {
            answer=''
        }
       
 }
    const handlequestion = (question,answer)=>{
        console.log(question,answer)
        let questionsanswers_copy = questionsanswers.slice();
        console.log(questionsanswers_copy);
        const currentQs = questionsanswers_copy.find((q) => q.question === question );
        currentQs.answer = answer;
        questionsanswers = questionsanswers_copy;
        console.log(questionsanswers,"questionsanswers")
        
    }
</script>
<style>

</style>
<p>
    from child component 
</p>
<form on:submit|preventDefault={()=>handlequestion(selected,answer)}>
    <select bind:value={selected} on:change={()=>handlechange(selected)} >
        {#each questionsanswers as {id,question,answer} }
            <option value={question}>
                {question}
            </option>
        {/each}
      
    </select>
   
        <textarea bind:value={answer} on:change={()=>handlequestion(selected,answer)}/>
       
   
     
    <button  type=submit>
		Submit
	</button>
    <p>{selected}</p>
</form>
