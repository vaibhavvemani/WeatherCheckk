<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	let country, localtime, temp, loc, lookinText, lookin;
	let slug = $page.params.slug
	onMount(async () => {
		const response = await fetch(
			`http://api.weatherapi.com/v1/current.json?key=58c018eea6c840be83c75506232204&q=${slug}`
		);
		const data = await response.json();
		loc = data.location.name;
		country = data.location.country;
		localtime = data.location.localtime;
		temp = data.current.temp_f;
		lookinText = data.current.condition.text;
		lookin = data.current.condition.icon;
	});

	let image;
	onMount(async () => {
		const response = await fetch(
			`https://api.pexels.com/v1/search?query=${$page.params.slug}&per_page=1`, {
				headers: {
					Authorization: "cjJlwujFm4RxUfLziINM2q9qFNiBir2hkAG4vHnzMfaRUbLQJ4f5aK08"
				}
			}
		);
		const data = await response.json();
		image = data.photos[0].src.portrait
	});
	let flag;
	onMount(async () => {
		const response = await fetch(
			`https://api.pexels.com/v1/search?query=${$page.params.slug} flag&per_page=1`, {
				headers: {
					Authorization: "cjJlwujFm4RxUfLziINM2q9qFNiBir2hkAG4vHnzMfaRUbLQJ4f5aK08"
				}
			}
		);
		const data = await response.json();
		flag = data.photos[0].src.portrait
	});
</script>

<div class="mainpage">
	<div class="backer">
		<img src={flag} alt="flag">
	</div>
	<div class="pastcontainer">
		<h1>VERGANGENHEIT</h1>
		<p>Highest tempreatures seen: </p>
		<p>Lowest Tempreatures seen: </p>
		<p></p>
	</div>
	<div class="countrycontainer">
		<div class="countryimage">
			<img src={image} alt="somethin">
			<div class="countryinfo">
				<h1 style="font-size: 60px;">{loc}</h1>
				<h4 style="font-size: 20px;">Coutry: {country}</h4>
				<h5 style="font-size: 16px;">Date: {localtime}</h5>
			</div>
		</div>
		<div class="presentcontainer">
			<div class="title-icon">
				<h1>JETZT</h1>
				<img src={lookin} alt="Condition" class="condition-icon">

			</div>
			<div class="presentinfo">
				<p>Temperature: {temp}F</p>
				<p>Condition: {lookinText}</p>
				<p></p>
			</div>
		</div>
	</div>
	<div class="futurecontainer">
		<h1>ZUKUNFT</h1>
	</div>
</div>

<style>
	.backer {
		height: 100%;
		width: 100%;
		position: absolute;
		z-index: -1;
		background-color: #2020208f;
	}
	.backer img {
		height: 100%;
		width: 100%;
		object-fit: cover;
	}
	.mainpage {
		height: 100vh;
		width: 100vw;
		color: black;
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 4rem;
	}
	.countrycontainer {
		height: 90%;
		width: 30%;
		margin: 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		/* justify-content: space-between; */
		gap: 10px;
		position: relative;
		
	}
	.countryimage:hover  .countryinfo{
		right: 0;
	}
	.countryimage {
		height: 30rem;
		width: 20rem;
		border: 8px solid black;
		padding: 10px;
		background-color: rgba(255, 255, 255, 0.3);
		border-radius: 10px;
		position: relative;
	}
	.countryimage img {
		height: 100%;
		width: 100%;
		border-radius: 5px;
		object-fit: cover;
		/* box-shadow: -6px -6px 7px #A1374D; */
	}
	.countryinfo {
		width: 100%;
		height: 100%;
		position: absolute;
		top: 0;
		right: -100%;
		background: #1f3d4738;
		backdrop-filter: blur(5px);
		/* padding-top: 10px;
		padding-left: 20px; */
		padding: 10px 20px;
		text-align: start;
		transition: 0.5s;
		color: white;
	}

	.pastcontainer {
		background-color: rgb(211, 220, 223);
		backdrop-filter: blur(4px);
		color: rgb(0, 0, 0);
		border-radius: 10px;
		height: 90%;
		width: 30%;
		padding: 10px;
	}

	.presentcontainer {
		background-color: rgb(211, 220, 223);
		backdrop-filter: blur(4px);
		color: rgb(0, 0, 0);
		border-radius: 10px;
		height: 45%;
		width: 100%;
		padding: 10px;
	}

	.title-icon {
		display: flex;
		align-items: center;
	}
	.futurecontainer {
		background-color: rgb(211, 220, 223);
		backdrop-filter: blur(4px);
		color: rgb(0, 0, 0);
		border-radius: 10px;
		height: 90%;
		width: 30%;
		padding: 10px;
	}
</style>
