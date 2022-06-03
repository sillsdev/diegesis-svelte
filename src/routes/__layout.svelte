<script>
	import Button from '@smui/button'
	import TopAppBar, { Row, Section, Title, AutoAdjust } from '@smui/top-app-bar'
	import IconButton from '@smui/icon-button'
	import Switch from '@smui/switch'
	import Banner from '@smui/banner'
	import Drawer, { AppContent, Scrim } from '@smui/drawer'
	import TextField from '@smui/textfield'
	import { Label, Icon } from '@smui/common'
	import { book, collection } from '../store.js'
import { debug, get_custom_elements_slots } from 'svelte/internal';

	let topAppBar
	let bannerOpen = false
	let drawerOpen = false

	$: console.log("drawerOpen: " + drawerOpen)

	let lightTheme =
		typeof window === 'undefined' || window.matchMedia('(prefers-color-scheme: light)').matches;
	function switchTheme() {
		lightTheme = !lightTheme;
		let themeLink = document.head.querySelector('#theme');
		if (!themeLink) {
			themeLink = document.createElement('link');
			themeLink.rel = 'stylesheet';
			themeLink.id = 'theme';
		}
		themeLink.href = `/smui${lightTheme ? '' : '-dark'}.css`;
		document.head
			.querySelector('link[href$="/smui-dark.css"]')
			?.insertAdjacentElement('afterend', themeLink);
	}
</script>

<TopAppBar bind:this={topAppBar} variant="static">
	<Row>
		<Section>
			<IconButton class="material-icons" on:click={() => drawerOpen = !drawerOpen}>menu</IconButton>
			<Title>{$book + " - " + $collection}</Title>
		</Section>
		<Section align="end" toolbar>
			<Title>Passage</Title>
			<Switch bind:checked={bannerOpen} icons={false} ></Switch>
		</Section>
	</Row>
</TopAppBar>
<Banner bind:open={bannerOpen}>
	<Icon slot="icon" class="material-icons">tune</Icon>
	<TextField variant="outlined" bind:value={$book} slot="actions">
		<Icon class="material-icons" slot="leadingIcon">search</Icon>
		<Icon class="material-icons" slot="trailingIcon">close</Icon>
	</TextField>
</Banner>
<Drawer variant="modal" fixed={false} bind:open={drawerOpen}>

</Drawer>
<Scrim fixed={false} />
<AppContent class="app-content">
	<AutoAdjust {topAppBar} style="display: flex; justify-content: space-between;">
		<div class="container"><slot /></div>
		<div class="container">
			<Button on:click={switchTheme}>
				<Label>{lightTheme ? 'Lights off' : 'Lights on'}</Label>
			</Button>
		</div>
	</AutoAdjust>
</AppContent>





<style>

</style>