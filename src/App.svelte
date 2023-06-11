<script>
	import { onMount } from "svelte";
	import "bootstrap/dist/css/bootstrap.min.css";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let gridData = [];
	let dataGrid;
  
	onMount(async () => {
	  const fetchData = async () => {
		try {
		  const response = await fetch(
			"https://api.recruitly.io/api/candidate?apiKey=TEST27306FA00E70A0F94569923CD689CA9BE6CA"
		  );
		  const responseData = await response.json();
		  jsonData = responseData.data;
  
		  gridData = jsonData.map((item) => ({
			id: item.id,
			firstName: item.firstName,
			email: item.email,
			mobile: item.mobile,
		  }));
  
		  renderDataGrid();
		} catch (error) {
		  console.error("Failed to fetch data:", error);
		}
	  };
  
	  fetchData();
	});
  
	const renderDataGrid = () => {
	  dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "id", width: 200 },
			{ dataField: "firstName", caption: "firstName", width: 200 },
			{ dataField: "email", caption: "email", width: 200 },
			{ dataField: "mobile", caption: "mobile", width: 150 },
			// Define other columns as needed
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
			texts: {
			  saveRowChanges: "Save",
			  cancelRowChanges: "Cancel",
			  deleteRow: "Delete",
			  confirmDeleteMessage: "Are you sure you want to delete this record?",
			},
		  },
		  paging: {
			pageSize: 20,
		  },
		  onRowInserting: async (e) => {
			console.log("Data being sent to API:", e.data);
			try {
			  const response = await fetch(
				"https://api.recruitly.io/api/candidate?apiKey=TEST27306FA00E70A0F94569923CD689CA9BE6CA",
				{
				  method: "POST",
				  headers: {
					"Content-Type": "application/json",
					
				  },
				  body: JSON.stringify(e.data),
				}
			  );
  
			  const responseData = await response.json();
			  if (response.ok) {
				e.data.id = responseData.id;
				gridData.push(e.data);
				dataGrid.refresh();
			  } else {
				console.error("Failed to add record:", responseData.error);
			  }
			} catch (error) {
			  console.error("Failed to add record:", error);
			}
		  },
               onRowUpdating: async (e) => {
		        try {
	           console.log(e);
	
                var newData = {
               id : e.key.id,
               firstName : e.newData.firstName === undefined ? e.oldData.firstName : e.newData.firstName ,
               surname : e.newData.surname=== undefined ? e.oldData.surname : e.newData.surname ,
               email : e.newData.email === undefined ? e.oldData.email : e.newData.email,
                mobile : e.newData.mobile === undefined ? e.oldData.mobile :e.mobile.email,
               }


               console.log(newData)
	          const response = await fetch(
		        `https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154`,
		{
		  method: "POST",
		  headers: {
			"Content-Type": "application/json",
		  },
		  body: JSON.stringify(newData),
		}
	  );
	  const responseData = await response.json();
	  if (response.ok) {
		const updatedItemIndex = gridData.findIndex((item) => item.id === e.key);
		gridData.push(e.newData);
		gridData[updatedItemIndex] = e.newData;
		dataGrid.refresh();
	  } else {
		console.error("Failed to update record:", responseData.error);
	  }
	} catch (error) {
	  console.error("Failed to update record:", error);
	}
  },
  }
	  );
	};
</script>
  
<div id="dataGrid"></div>
