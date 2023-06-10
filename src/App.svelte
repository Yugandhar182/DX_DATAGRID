<script>
	import { onMount } from "svelte";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let data = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
  
	  const gridData = jsonData.map((item) => ({
		id: item.reference,
		firstName: item.fullName,
		surname: item.fullName,
		email: item.email,
		mobile: item.mobile,
	  }));
  
	  createDataGrid(gridData);
	});
  
	function createDataGrid(gridData) {
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "ID" },
			{ dataField: "firstName", caption: "First Name" },
			{ dataField: "surname", caption: "Surname" },
			{ dataField: "email", caption: "Email" },
			{ dataField: "mobile", caption: "Mobile" },
		  ],
		  showBorders: true,
		  filterRow: {
			visible: true,
		  },
		  editing: {
			allowDeleting: true,
			allowAdding: true,
			allowUpdating: true,
			mode: "popup",
			form: {
			  labelLocation: "top",
			},
			popup: {
			  showTitle: true,
			  title: "Row in the editing state",
			},
			onRowInserted: async (e) => {
			  const newRowData = e.data;
			  console.log(newRowData);
			  try {
				const response = await fetch(
				  "https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154",
				  {
					method: "POST",
					headers: {
					  "Content-Type": "application/json",
					},
					body: JSON.stringify(newRowData),
				  }
				);
  
				console.log(newRowData);
				if (response.ok) {
				  // Handle success
				  console.log("New row added successfully");
				} else {
				  // Handle error
				  console.error("Failed to add new row");
				}
			  } catch (error) {
				// Handle error
				console.error("Failed to add new row", error);
			  }
			},
			onRowUpdated: async (e) => {
			  const updatedRowData = e.data;
			  console.log(updatedRowData);
			  try {
				const response = await fetch(
				  `https://api.recruitly.io/api/candidate/${updatedRowData.id}?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154`,
				  {
					method: "POST",
					headers: {
					  "Content-Type": "application/json",
					},
					body: JSON.stringify(updatedRowData),
				  }
				);
  
				
  
				if (response.ok) {
				  // Handle success
				  console.log("Row updated successfully");
				} else {
				  // Handle error
				  console.error("Failed to update row");
				}
			  } catch (error) {
				// Handle error
				console.error("Failed to update row", error);
			  }
			},
			onRowRemoved: async (e) => {
			  const removedRowData = e.data;
			  console.log(removedRowData);
			  try {
				const response = await fetch(
				  `https://api.recruitly.io/api/candidate/${removedRowData.id}?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154`,
				  {
					method: "DELETE",
				  }
				);
  
				
  
				if (response.ok) {
				  // Handle success
				  console.log("Row deleted successfully");
				} else {
				  // Handle error
				  console.error("Failed to delete row");
				}
			  } catch (error) {
				// Handle error
				console.error("Failed to delete row", error);
			  }
			},
		  },
		  paging: {
			pageSize: 10,
		  },
		  pager: {
			showPageSizeSelector: true,
			allowedPageSizes: [5, 10, 20],
			showInfo: true,
		  },
		}
	  );
	}
  </script>
  
  <div id="dataGrid"></div>
  