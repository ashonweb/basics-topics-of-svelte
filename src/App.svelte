<script>
import { onMount } from "svelte";


import InfoObjects from "./Info_Objects.svelte";
import Nested from "./Nested.svelte";
import Select from "./Select.svelte";
let githubRepoInfoPromise;
let value='bhagya';
 let selectedradio = 1;
 let selectedcheck = ['a'];
 let checkboxes = ['b','a','v','e']
	let yes=false;
	let a =1;
	$:wordl = 'sadasd'
	let name ="bhagya";
	let count = 0;
	function handleClick () {
		count = count+1;
	}
	$:double = count*2;
	$: if(count >= 9) {
		alert("count is more than 9 or equal");
		
		count = 0;
	}
 	let number = [1,2,3]
	function add () {
		 number.push(number.length+1)		
	}
	function addoneway () {
		 number.push(number.length+1);
		 number=number	
	}
	function addsway () {
		 number = [...number , number.length+1]	
	}
	$: sum = number.reduce((a,b)=>a+b,0);
	let obj = {
		name:"bhagya",
		age:"23",
		addredd:"ext",

	}
	let userlogin = {
		login:false,
	}
	function toggle () {
		userlogin.login = !userlogin.login
	}
	let cats = [{
		catname:'katie',
		country:'india'
	},
	{
		catname:'yellow',
		country:'aus'
	},{
		catname:'red',
		country:'us'
	},{
		catname:'saby',
		country:'nz'
	}]
	
	const loadRepoInfo = async () =>{
		const response = 		await fetch('https://protected-ridge-21819.herokuapp.com/characters')

		if (response.status === 200) {
		return await response.json();
		} else {
		throw new Error(response.statusText);
		}
		// await fetch('https://protected-ridge-21819.herokuapp.com/characters')
		// .then(res=>res.json())
		// .then( res=>{
		// 	console.log(res)
		// 	return  res
		// })
		// .catch(err=>{
		// 	return err
		// })
			
	
	}
	
	const handleapi =  () => {
		githubRepoInfoPromise = loadRepoInfo();
		
	
	}
	
	function handleevent (e) {
		alert(e.clientX,'on mouse over' )
	}
	$: result=undefined;
	onMount (async ()=>{
		await fetch('https://protected-ridge-21819.herokuapp.com/characters')
		.then(res=>res.json())
		.then( res=>{
			console.log(res)
			result= res;
		})
		.catch(err=>{
			return err
		})
	})
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<Nested name={name}/>
	<Nested/>
	<button on:click={handleClick}>
		clicked these many times {count} and double is {double}
	</button>
	<p> Array wont get updated automatically</p>
	<p>{number}</p>
	<button on:click={add}>
		add
	</button>
	<p>Two ways of doing ...clarify with code</p>
	<button on:click={addoneway}>
		add one way
	</button>
	<button on:click={addsway}>
		add  second way
	</button>
	<p> total sum is {sum}</p>
	<InfoObjects {...obj}/>
	<hr/>
	conditional statement inside html elements only rather than function
	<!-- # indicates block opening / indicates block closing -->
	<!-- <p>
		{#if userlogin.login }
		<button on:click={toggle}>
			click to Logout
		</button>
		{/if}


		{#if !userlogin.login}
		<button on:click={toggle}>
			click to login
		</button>
		{/if}
	</p> -->
	<!-- # indicates block opening :indicades block continuation {:else} / indicates block closing -->
	<p>
		{#if userlogin.login }
		<button on:click={toggle}>
			click to Logout
		</button>
		{:else}
		<button on:click={toggle}>
			click to login
		</button>
		{/if}
	</p>
	<hr/>
	Looping array or array-like object (i.e. it has a length property)
	<ul>
		{#each cats as {catname,country} ,i }
		<li>
			<p>Cat {catname} belongs to country {country}</p> 
		</li>
		{/each}
	</ul>
	<hr/>
	fetching api's handling asyn data
	<button on:click={handleapi}>
		get characters
	</button>
	{#await githubRepoInfoPromise}
		<p>waiting </p>
	{:then apiResponse } 
		{#if  Array.isArray(apiResponse)}
		{#each apiResponse as {name}}
		<p>{name}</p>
	{/each}
	{:else}
	<p></p>
		{/if}
		
	{:catch err}
	<p>
		{err}
	</p>	
	{/await}
	<hr/>
	<!-- <div on:mouseover={handleevent}>
		mouce over to listen on the element done by on: directive
	</div>
	<hr/> -->
	<p> input  type text bind:value directive</p>
	<input type="text"  bind:value={wordl}/>
	<p>{wordl}</p>
	<p>Type number and range</p>
	<input type="number" bind:value={a} min=0 max=12/>
	<input type=range bind:value={a} min=0 max=12/>
	<p>Type checkbox</p>

	<input type="checkbox"  bind:checked={yes}/>
	<button disabled={!yes}>dad</button>
	<p> multiple inputs relating to the same value, you can use bind:group along with the value attribute. Radio inputs in the same group are mutually exclusive; checkbox inputs in the same group form an array of selected values.</p>
	<input type=radio bind:group={selectedradio} value={1}/>
	<input type=radio bind:group={selectedradio} value={2}/>
	<input type=radio bind:group={selectedradio} value={3}/>
	{#each checkboxes as checkbox }
		<label>
			<input type="checkbox" bind:group={selectedcheck} value={checkbox}/> {checkbox}</label>
	{/each}
	<textarea bind:value></textarea>
	<hr/>
	<p>Select</p>
	<Select/>
	<hr/>
	<p>Life cycle methods</p>
	 
	{#if  Array.isArray(result)}
	{#each result as {name}}
	<p>{name}</p>
{/each}
{:else}
<p>Loading</p>
{/if}


</main>