<script>
	import data from '$lib/data.json';
	const scoreList = {
        "3": 99428,
        "4": 93003,
        "5": 86577,
        "6": 80152,
        "7": 73814,
        "8": 68533,
        "9": 63253,
        "10": 57972,
        "11": 52691,
        "12": 46374,
        "13": 39784,
        "14": 33193,
        "15": 26603,
        "16": 33193,
        "17": 39784,
        "18": 46374,
        "19": 52691,
        "20": 57972,
        "21": 63253,
        "22": 68533,
        "23": 73814,
        "24": 80152,
        "25": 86577,
        "26": 93003,
        "27": 99428
    };

	const userList = [ 
			{
                name: "Jan",
                photo: "avatar/Jan.png"              
            },
            {
                name: "Lies",
                photo: "avatar/Lies.png"             
            },
            {
                name: "Mieke",
                photo: "avatar/Mieke.png"             
            },
            {
                name: "Anouk",
                photo: "avatar/Anouk.png"             
            },
            {
                name: "Jeronimo",
                photo: "avatar/Jeronimo.png"             
            },
            {
                name: "Fleur",
                photo: "avatar/Fleur.png"              
            },
            {
                name: "Juno",
                photo: "avatar/Juno.png"              
            },
            {
                name: "Pepijn",
                photo: "avatar/Pepijn.png"              
            },
            {
                name: "Charlie",
                photo: "avatar/Charlie.png"              
            },
            {
                name: "Karel",
                photo: "avatar/Karel.png"              
            },
            {
                name: "Bas",
                photo: "avatar/Bas.png"              
            },
            {
                name: "Liam",
                photo: "avatar/Liam.png"              
            },
            {
                name: "Marco",
                photo: "avatar/Marco.png"             
            },
            {
                name: "Joanne",
                photo: "avatar/Joanne.png"             
            },
            {
                name: "Miriam",
                photo: "avatar/Miriam.png"             
            },
            {
                name: "Sem",
                photo: "avatar/Sem.png"              
            },
            {
                name: "Noor",
                photo: "avatar/Noor.png"             
            },
            {
                name: "Yara",
                photo: "avatar/Yara.png"             
            },
            {
                name: "Willem",
                photo: "avatar/Willem.png"
            },
            {
                name: "Kahim",
                photo: "avatar/Kahim.png"
            }
        ]

	let groepen = $state([{title:"Gevangenen",slug: "gevangenen"}, {title:"LGHBTQI+",slug:"lhbtq+"},{title:"Inheemse groepen",slug:"inheemse"},{title:"Immigranten",slug:"migranten"}]);
	let selected = $state();
	let title = $state(null);
	let photo = $state(null);
	let text = $state(null);
	let score = $derived(title?.score + photo?.score + text?.score);
	let likes = $derived(scoreList[score])
	let comments = $derived(data[selected]?.contentItems.filter( item => item.type == 'comment').filter(() => likes != null).filter(c => 
            (c.about == 'title' && c.score <= title?.score && c.maxScore >= title?.score) ||
            (c.about == 'photo' && c.score <= photo?.score && c.maxScore >= photo?.score) ||
            (c.about == 'text' && c.score <= text?.score && c.maxScore >= text?.score))
			.sort(() => Math.random() - 0.5).splice(0,3));
	let users = $derived(userList.filter(() => title?.score + photo?.score + text?.score ).sort(() => Math.random() - 0.5).splice(0,3));
	let showSelection = $state(true);
	function reset() {title = null; photo = null; text = null;showSelection = true;}

</script>

<svelte:head>
	<title>Like! nakijktool</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
		<form>
			<h1 style="margin: 0;">Nakijktool</h1>
			<select bind:value={selected} onchange={reset} style="width: 100%; display: block;float:none;margin:auto;"> 
				<option value={null}> Kies een casus</option>
				{#each groepen as groep}
					<option value={groep.slug}> {groep.title} </option>
				{/each}
			</select>
		</form>
		{#if selected != null  }
		<h3 class="group">Jullie post over {data[selected].name}</h3>
		<h4 class="group">van: _______________________</h4>
		<div class="post" >
			<div class="post-title"> 
				{title?.value || ""}<br>
				{#if showSelection}<select bind:value={title}>
					<option value={null} selected> Kies een titel</option>
					{#each data[selected].contentItems.filter( item => item.type == 'title') as item}
						<option value={item}> {item.value} </option>
					{/each}
				</select> <br/> {/if}
			</div>
			<div class="post-body">
					{#if photo == null}
						<div class="imagePicker">
						{#each data[selected].contentItems.filter( item => item.type == 'photo') as item}
							<img src={item.value} alt="Selecteer de juiste foto, zie hier {item.value}" class="imagePick" onclick={() => photo = item} style="max-height:10em;">
						{/each}
						</div>
					{:else }
						<img src={photo.value} alt="foto bij post van {selected}"><br>
						{#if showSelection} <button onclick={() => photo = null}>kies andere foto</button> {/if}
					{/if}
				<p> 
					{#if showSelection}<select bind:value={text}>
						<option value={null} selected> Kies een text</option>
						{#each data[selected].contentItems.filter( item => item.type == 'text') as item}
							<option value={item}> {item.value} </option>
						{/each}
					</select> <br> {/if}
					<br>{text?.value || ""}
				</p>
			</div>
			<div class="post-actions">
				<div class="post-likes"> ❤️ {likes || 0} Likes</div>
				<div class="post-comments" > &#128172; {comments.length} Comments</div>
			</div>
			<div class="comments" style="display: block;">
				{#each comments as comment,i}
					<div class="comment">
						<img src="/{users[i].photo}" alt="user profile picture {users[i].name}">
						<span class="comment-username">{users[i].name}</span>
						<p> {comment.value} </p>
					</div>
				{/each}
			</div>
		</div>
		{#if likes}
			<div style="display:flex"><button onclick={reset}>Wis post</button><button onclick={() => window.print() }>Print post</button><button onclick={() => showSelection = !showSelection}> {#if showSelection } Verberg {:else} Toon {/if} bewerk knoppen</button>  </div>
		{/if}
		{/if}
	
</section>

<style>

	.group {
			display: none;
		}

	@media print {
		button,select,form {
			display: none;
		}
		.group {
			display: block;
		}
		.post, .comment {
			border-style: solid;
			border-color: black;
		}
		:root {
			font-size: 10px;
			color:black !important;
		}
		img{
			max-height: 200px !important;
		}

        .comment {
            color: black !important;
            background-color: white !important;
        }

        .comment-username {
            color:black !important;
        }
	}


	.imagePicker {
		display: flex;
		overflow:scroll;
		gap:1rem;
		padding: 1rem;
	}

	.imagePicker img {
		cursor:pointer;
	}

	select {
		width: 15em;
		float: right;
		margin: 0.8rem;
	}

	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

:root{ 
    --p1: #009CEF;
    --p2: #FFFFFF;
    --p3: #CC0000;
    --p4: #000000;

    --s1: #666d70;
    --s2: #772D6B;
    --s3: #FFcF00;
    --s4: #60b669;
    font-family: sans-serif;
}

img {
	object-fit: contain;
}

button:disabled{
    color:inherit;
    background-color: inherit;

}

button{
    border-radius: 5px;
    border-width: medium;
    border-style: solid;
    border-top: 0;
    border-left: 0;
    border-right: 0;
    border-color: var(--s1);
    font-size: 1rem;
    padding: 0.5rem;
    margin: 0.3rem;
    background-color: rgba(180, 180, 180, 0.4);
    text-align: center;
   -webkit-backdrop-filter: blur(5px);
    backdrop-filter: blur(5px);
}

button {
    font-weight: bold;
    background-color: var(--p3);
    color: var(--p2);
}

.post{
    border-radius: 10px;
    overflow: hidden;
    min-width: 20ch;
    margin: 0.8rem;
    /* flex: 1; */
    /* flex-basis: 35ch; */
    max-width: 60ch;
}

.post-title {
    padding:0.5rem;
    font-size: 1.8rem;
    text-align: center;
}

.post-title {
    font-weight: normal;
    font-family: verdana;
}

.post-body {
    display: flex;
    flex-direction: column;
    width: 100%;
}

.post-body button {
    width: fit-content;
    padding-inline: 1rem;
    align-self: center;
    margin-block: 0;
}

.post-body p {
    margin: 1rem;
}

.post-body img {
    max-width: 100%;
    max-height: 50vh;
    margin: auto;
}

.post {
    background-color: var(--p2);
    color: var(--p4);
}

.post-actions {
    display: block;
    font-size: 2em;
    margin: 5px;
}

.post-actions div {
    display: inline-block;
}

.post-likes:hover {
    color:var(--p3);
}

.post-comments:hover {
    color:var(--s3);
}

.comment {
    overflow: hidden;
    border-radius: 5px;
    color: var(--p2);
    background-color: rgba(0, 0, 0, 0.5);
    margin: 0.5rem;
    padding: 0.5rem;
}

.comment img {
    float: left;
    height: 3rem;
    margin: 0.5rem;
}

.comment-username {
    font-weight: bold;
    color:var(--s3)
}

.comment p {
    margin-block: 1px;
}

</style>
