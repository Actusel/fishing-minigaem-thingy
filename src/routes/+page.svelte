<script>
	let timing_arrow;
	let green_timing;
	let greenWidth = "75px";
	let inventoy_toggled = false;
	let shop_toggled = false;
	let fish_caught = [];
	let money = 100;

	function onKeyDown(event) {
		if (event.key == " ") {
			if (fisherman_state.name == "idle") {
				fishing();
			} else if (fisherman_state.name == "catching") {
				fish_catching();
			}
		} else if (event.key == "Escape") {
			inventoy_toggled = false;
			shop_toggled = false;
		} else if (event.key == "Enter") {
			if (inventoy_toggled) {
				sell_all();
			}
		} else if (event.key == "e") {
			toggleInventory();
		} else if (event.key == "b") {
			toggleShop();
		}
	}
	
	let rods = [
		{
			name: "wooden rod",
			price: 0,
			image: "wooden_rod.png",
			size: 10,
			time: 1,
			tier: 1,
			bought: true
		},
		{
			name: "iron rod",
			price: 500,
			image: "iron_rod.png",
			size: 15,
			time: 0.8,
			tier: 2,
			bought: false
		},
		{
			name: "golden or blaze rod idk",
			price: 2000,
			image: "golden_or_blaze_rod_idk.png",
			size: 20,
			time: 0.4,
			tier: 3,
			bought: false
		},
		{
			name: "blue blunderbuss",
			price: 5000,
			image: "Blue Blunderbuss.png",
			size: 50,
			time: 0.6,
			tier: 4,
			bought: false
		}
	];
	let rod_equipped = rods[0];

	let fish = [
		{
			name: "Tuna",
			image: "https://ipnlf.org/wp-content/uploads/2021/08/Skipjack.png",
			size: 15,
			price: 4,
			tier: 1
		},
		{
			name: "Salmon",
			image: "https://fishing.tas.gov.au/ContentImages/Salmon---Atlantic.png",
			size: 10,
			price: 6,
			tier: 1
		},
		{
			name: "Trout",
			image: "https://content.osgnetworks.tv/flyfisherman/content/photos/AboutRainbowTrout-HERO-1200x800.jpg",
			size: 8,
			price: 7,
			tier: 1
		},
		{
			name: "Bass",
			image: "https://midatlanticstocking.com/wp-content/uploads/Largemouth_bass-scaled.jpg",
			size: 12,
			price: 5,
			tier: 1
		},
		{
			name: "Catfish",
			image: "https://fishspecies.dnrec.delaware.gov/imageDB.ashx?id=30&ss=2",
			size: 15,
			price: 5,
			tier: 2
		}
		,
		{
			name: "Goldfish",
			image: "https://i0.wp.com/aquariumtidings.com/wp-content/uploads/2014/06/Depositphotos_5125679_original.jpg?fit=4848%2C4000&ssl=1",
			size: 2,
			price: 30,
			tier: 3
		},
		{
			name: "Pike",
			image: "https://www.ifiske.se/img/species/large/gadda.jpg",
			size: 12,
			price: 6,
			tier: 1
		},
		{
			name: "Mackerel",
			image: "https://www.fisheries.noaa.gov/s3//styles/original/s3/2022-09/640x427-Mackerel-Pacific-NOAAFisheries.png?itok=eF0gw4nM",
			size: 6,
			price: 12,
			tier: 1
		},
		{ 
			name: "gyrados",
			image: "https://oyster.ignimgs.com/mediawiki/apis.ign.com/pokemon-blue-version/e/e4/Gyarados.jpg",
			size: 25,
			price: 9.9,
			tier: 4
		},
		{ 
			name: "tralalero tralala",
			image: "https://i5.walmartimages.com/asr/01830384-d9f8-42d3-a44b-568651bf7540.0cbd64cca6abc7b7076228cd2c467e4e.jpeg?odnHeight=768&odnWidth=768&odnBg=FFFFFF",
			size: 20,
			price: 13,
			tier: 4
		},
		{
			name: "swordfish",
			image: "image-removebg-preview (13).png",
			size: 25,
			price: 11,
			tier: 2
		},
		{
			name: "shoe",
			image: "https://t4.ftcdn.net/jpg/00/65/05/55/360_F_65055516_gwG0rdTkvj9WLqnu1pkC3DbMkrAERRsx.jpg",
			size: 5,
			price: 1,
			tier: 1
		},
		{
			name: "transgender fish",
			image: "https://pbs.twimg.com/media/Fq2rci0aUAAUVfX.jpg",
			size: 1.1,
			price: 100,
			tier: 3
		}
	];
	let fisherman_states = [
		{ name: "idle",
		image: "boat_man_epic-removebg-preview.png",
		},
		{ name: "fishing",
			image: "boat_man_epic-removebg-preview.png"
		},
		{ name: "catching",
		image: "boat_man_epic_catching-removebg-preview.png"
		}

	];
	let fisherman_state = fisherman_states[0];

	function toggleInventory() {
		inventoy_toggled = !inventoy_toggled;
	}

	function toggleShop() {
		shop_toggled = !shop_toggled;
		inventoy_toggled = false;
	}
	function sell(fish) {
		money += fish.price;
		fish_caught = fish_caught.filter(f => f !== fish);
		alert("You sold the " + fish.name + " for " + fish.price + " coins!");
	}
	function sell_all() {
		let total_price = 0;
		for (let i = 0; i < fish_caught.length; i++) {
			total_price += fish_caught[i].price;
		}
		money += total_price;
		fish_caught = [];
		if (total_price == 0) {
			alert("You don't have any fish to sell!");
			return;
		} else {
			alert("You sold all your fish for " + total_price + " coins!");
		}
	}

function fishing() {
	fisherman_state = fisherman_states[1];
	let random_time = Math.floor(Math.random() * 5000*rod_equipped.time) + 2000*rod_equipped.time;
	setTimeout(() => {
		fisherman_state = fisherman_states[2];
	}, random_time);
}

function buy(rod) {
	if (rod.price <= money) {
		money -= rod.price;
		rod.bought = true;
		alert("You bought the " + rod.name + " for " + rod.price + " coins!");
		rods=rods;
	} else {
		alert("You don't have enough money to buy this rod!");
	}
}

function equip(rod) {
		rod_equipped = rod;
}



function fish_catching() {
	if (fisherman_state.name == "catching" && timing_arrow && green_timing) {
		let random_fish_number = Math.floor(Math.random() * fish.length);
		let random_fish = { ...fish[random_fish_number]};
		if (random_fish.tier > rod_equipped.tier) {
			fish_catching();
			return;
		}
		let random_multiplier = ((Math.random()+1)**5)/30 + 1;
		random_fish.size = Math.floor(random_multiplier * random_fish.size * (rod_equipped.size/10));
		random_fish.price = Math.floor(random_fish.price * random_fish.size);
		greenWidth = (75-(random_fish.size**0.7)) + "px";
		let timing_arrow_position = timing_arrow.getBoundingClientRect();
		let green_timing_position = green_timing.getBoundingClientRect();
		let timing_arrow_center = timing_arrow_position.left + (timing_arrow_position.width / 2);
		if (timing_arrow_center >= green_timing_position.left-10 && timing_arrow_center <= green_timing_position.right+10) {
			fisherman_state = fisherman_states[0];
			
			fish_caught.push(random_fish);
			fish_caught=fish_caught
		} else {
			fisherman_state = fisherman_states[0];
			alert("¯\\_( ͡° ͜ʖ ͡°)_/¯");
			
		}
	}
	
}


function average_catch_value_from_rod(rod) {
	let total_value = 0;
	for (let i = 0; i < fish.length; i++) {
		total_value += fish[i].price * fish[i].size * (rod.size/10);
	}
	return (total_value / fish.length).toFixed(2);
}
</script>

<svelte:window on:keydown|preventDefault={onKeyDown} />

<aside style="position: absolute; top: 0; left: 0; width: 100vw; height: 100vh;" >
	
	<button on:click={toggleInventory} id="button_toggle" style="left:10px;">Toggle Inventory</button>

	<div class=hide class:fish_table={inventoy_toggled}>
		<button on:click={sell_all} style="background-color:green; border-radius:10px; color:white; height: 25px; padding-left: 10px; padding-right:10px; margin-right:100%;">Sell⠀all</button>
	{#each fish_caught as fishy}
		<div class="fish">
			<img src={fishy.image} style="width:100px; max-height:75px;" alt={fishy.name} />
			<p>{fishy.name}</p>
			<p>Size: {fishy.size}</p>
			<p>Price: {fishy.price}</p>
			<button on:click={() => sell(fishy)} style="background-color:green; border-radius:10px; color:white;">Sell</button>
		</div>
	{/each}
		
	</div>
	<p style="position: absolute; top: 10px; left: 160px; z-index: 1; color: white; font-size: 20px;">
		money: {money}
	</p>
</aside>

<aside style="position: absolute; top: 0; right: 0; width: 100vw; height: 100vh;">
	<button on:click={toggleShop} id="button_toggle" style="right:10px;">Toggle Shop</button>
	
	<div class=hide class:shop_table={shop_toggled} style="overflow: hidden;">
		<img src="shop_background.png" alt="shop" class=shop_table style="width: 100vw; height: 100vh; top: 0; left: 0; z-index: 2; object-fit: cover;">
		{#each rods as rod}
			<div class="rod">
				<img src={rod.image} alt={rod.name} />
				{rod.name} <br>
				Price: {rod.price}
				<br>
				{#if rod.bought}
					(Owned)
					<button class="buy_button" style="background-color:lightseagreen" on:click={() => equip(rod)}>Equip</button>
				{:else}
				
					<button class="buy_button" on:click={() => buy(rod)}>Buy</button>
				
					
				{/if}
			</div>
		{/each}
		<div id=equipped>
			<p style="color: black; font-size: 20px;">Equipped Rod:</p>
				<img src={rod_equipped.image} alt={rod_equipped.name} />
				<p>{rod_equipped.name}</p>
				catch size: {rod_equipped.size} <br> <p style=font-size:small;font-style:italic;text-align:center;background-color:white;border-radius:30%;>disclaimer: <br> larger fishes mean harder catches</p>
				<p>speed: {1/rod_equipped.time}</p>
				<p>average catch value: {average_catch_value_from_rod(rod_equipped)}</p>
		</div>

	</div>
		



</aside>

<main>
	<img src={fisherman_state.image} alt="fisher-man" id="fisher">
	<img src={rod_equipped.image} alt="fishing-rod" id="rod" class:fishing={fisherman_state.name == "fishing"} class:catch={fisherman_state.name == "catching"} >

	{#if fisherman_state.name == "idle"}
	<button on:click={fishing} >start fishing</button>
	{/if}
	{#if fisherman_state.name == "fishing"}
	<button disabled>fishing...</button>
	{/if}
	{#if fisherman_state.name == "catching"}
	<button on:click={fish_catching}> CATCH!</button>
	<div id="timing">
		<div id=red></div>
		<div id=green bind:this={green_timing} style="width:{greenWidth}"></div>
		<img bind:this={timing_arrow} src="https://pngmaterial.com/dvsxyz02/uploads/blue-arrow-png.png" alt="timing arrow" id="arrow">
		
		 </div>
	
	{/if}
	
</main>

<footer> controls: SPACEBAR to fish, E for Inventory, B for shop, ENTER to sell all, ESC to close inventory/shop </footer>

<div>
	<img src="https://img.freepik.com/free-vector/nature-underwater-blue-clear-water-background_1308-129993.jpg" alt="" id="background">
</div>

<style>
	.rod button:hover {
		background-color: darkolivegreen;
	}
	.rod button {
		background-color: darkgreen; margin-left:auto; padding-right: 10px; padding-left: 10px;
	}
	.fishing {
		transform: rotate(-45deg);
	}
	.catch {
		transform: rotate(30deg);
	}
 main {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	height: 100vh;
	z-index: 1;
	}
	footer{
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100vw;
		height: 50px;
		background-color: rgba(0, 0, 0, 0.5);
		color: white;
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 1;
	}
	main img {
		position: absolute;
		max-width: 300px;
	}
	#fisher {
		position: absolute;
		max-width: 300px;
		z-index: 1;
		margin-bottom: 100px;
	}
	#rod {
		position: absolute;
		transform-origin: 65% 70%;
		max-width: 100px;
		margin-right:70px;
		z-index: 1;
		margin-bottom: 110px;
	}
	#button_toggle {
		position: absolute;
		z-index: 1;
		background-color: navy;
		color: white;
		border: none;
		border-radius: 5px;
		padding: 10px;
		top: 7px;
	}
	main button{
		position: absolute;
		display:flex;
		width: 600px;
		height: 400px;
		z-index: 1;
		background-color: rgb(0,0,0,0.1);
		border-radius: 5px;
		top: 20%; 
		justify-content: center;
		padding-top:70px;
	}
	#background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: -1;
	}
	#timing > * {
		position: absolute;
		height: 75px;
		width: 75px;
		top: 60%;
		transform: translate(-50%, -50%);
		z-index: 1;
	}
	.hide {
		display: none;
	}
	
	.fish_table {
		left: 0;
		top: 60px;
		position: absolute;
		max-width: 30vw;
		min-width: 10vw;
		z-index: 3;
		background-color: rgba(255, 255, 255);
		display:flex;
		flex-wrap: wrap;
		border-radius: 10px;
		padding: 10px;
		color: dimgray;
		max-height: 80vh;
		overflow-y: auto;
		overflow-x: hidden;
	}
	.shop_table {
		left: 0;
		top: 60px;
		position: absolute;
		width: 100vw;
		height: 92.5vh;
		z-index: 2;
		background-color: rgba(255, 255, 255);
		display:flex;
		flex-direction: column;
		overflow: hidden;
	}
	.rod {
		display: flex;
		margin: 10px;
		width: 300px;
		border-width: 5px;
		background-color: #938f61;
		z-index: 3;
		border-color: #573e1a;
		border-radius: 10px;
	}
	#equipped {
		position: absolute;
		top: 0;
		right: 0;
		width: 200px;
		height: auto;
		z-index: 2;
		background-color: lavenderblush;
		border-bottom-left-radius: 20%;
		display:flex;
		flex-direction: column;
		padding: 10px;
		color: darkslategray;
		text-align: end;
	}

	.rod img {
		height: 100px;
		width: 100px;
		margin-right: 10px;
	}

	.fish{
		border-width: 1px;
		border-bottom-right-radius: 50%;
		border-bottom-left-radius: 50%;
		text-align: center;
		margin-bottom: 10px;
		padding-bottom: 15px;
		margin-right: 5px;
	}
	#green {
		background-color: green;
	}
	#red {
		background-color: red;
		width: 350px;
	}
	#arrow {
		
		margin-top: 30px;
		transform: rotate(-90deg);

	}

	@keyframes arrow {
	0% {
		margin-left: 0px;
	}
	25% {
		margin-left: 140px;
	}
	75% {
		margin-left: -215px;
	}
	100% {
		margin-left: 0px;
	}
	}
	#arrow {
		animation: arrow 1.3s linear infinite;
	}

	@media (max-width: 600px) {
	main button{
		width: 100vw;
		height: 60vh;
		top: 20%;
		font-size: 20px;
	}

		.fish_table {
			max-width: 50vw;
			min-width: 10vw;
			background-color: rgba(255, 255, 255);
		}
		footer{
			display: none;
		}
		.rod{
			width: 250px;
		}
	}
</style>