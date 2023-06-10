<script>
	import { onMount } from "svelte";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let data = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
	  console.log(jsonData, "json");
  
	  const gridData = jsonData.map((item) => ({
		id: item.id,
		firstName: item.firstName,
		surname: item.surname,
		email: item.email,
		mobile: item.mobile,
	  }));
  
	  console.log(gridData, "griddata");
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "ID" },
			{ dataField: "firstName", caption: "firstName" },
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
			  try {
				const response = await fetch(
				  "https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E",
				  {
					method: "POST",
					headers: {
					  "Content-Type": "application/json",
					},
					body: JSON.stringify(newRowData),
				  }
				);
  
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
			  try {
				const response = await fetch(
				  `https://api.recruitly.io/api/candidate/${updatedRowData.id}?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E`,
				  {
					method: "PUT",
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
	});
  </script>
  
  <div id="dataGrid"></div>
  