<script>
	import { DataGrid, Column } from 'devextreme-svelte';
	import { onMount } from 'svelte';
  
	let candidates = [];
  
	onMount(async () => {
	  try {
		const response = await fetch('https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E');
		candidates = await response.json();
	  } catch (error) {
		console.error('Error fetching data:', error);
	  }
	});
  </script>
  
  <DataGrid dataSource={candidates} showBorders={true}>
	<Column dataField="id" caption="ID" dataType="number"></Column>
	<Column dataField="firstname" caption="First Name"></Column>
	<Column dataField="surname" caption="Surname"></Column>
	<Column dataField="email" caption="Email"></Column>
	<Column dataField="mobile" caption="Mobile"></Column>
  </DataGrid>
  