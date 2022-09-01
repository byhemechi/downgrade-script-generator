<script lang="ts">
	let manifestId = '649695825989344999';
	let username = '';

	let script: string;
	$: script = `cd $env:TEMP;
rm -r steamcmd
mkdir steamcmd;
cd steamcmd;
curl https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip -o steamcmd.zip;
tar -xf steamcmd.zip;

./steamcmd +login ${username} +download_depot 620980 620981 ${manifestId} +exit;

explorer .\\steamapps\\content\\app_620980\\depot_620981`;

	let scripturl: string;
	// @ts-ignore
	$: scripturl = `data:text/plain;base64,${btoa(script)}`;
</script>

<div class="prose my-8 mx-auto">
	<table>
		<tbody>
			<tr>
				<td>
					<label for="manifest"
						>Manifest ID (from <a href="https://steamdb.info/depot/620981/history/"
							>Beat Saber Manifest History</a
						>)</label
					>
				</td>
				<td> <input bind:value={manifestId} id="manifest" /></td>
			</tr>
			<tr>
				<td><label for="username">Steam Username</label></td>
				<td><input type="text" id="username" placeholder="Username" bind:value={username} /></td>
			</tr>
		</tbody>
	</table>
	{#if username && manifestId}
		<a href={scripturl} download="downgrade.ps1" class="appearance-[button]">Download Script</a>
		<pre>{script}</pre>
	{:else}
		<p>Please fill in all fields</p>
	{/if}
</div>
