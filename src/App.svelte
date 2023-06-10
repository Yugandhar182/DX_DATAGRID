<script>
	import { onMount } from "svelte";
	import "bootstrap/dist/css/bootstrap.min.css";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let gridData = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
  
	  gridData = jsonData.map((item) => ({
		id: item.id,
		firstName: item.firstName,
		surname: item.surname,
		email: item.email,
		mobile: item.mobile,
	  }));
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(document.getElementById("dataGrid"), {
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
		  texts: {
			saveRowChanges: "Save",
			cancelRowChanges: "Cancel",
			deleteRow: "Delete",
			confirmDeleteMessage: "Are you sure you want to delete this record?",
		  },
		},
		paging: {
		  pageSize: 10,
		},
		onRowInserting: async (e) => {
		  const newData = {
			// Provide the new record data
			firstName: e.data.firstName,
			surname: e.data.surname,
			email: e.data.email,
			mobile: e.data.mobile,
		  };
  
		  try {
			const response = await fetch("https://api.recruitly.io/api/candidate", {
			  method: "POST",
			  headers: {
				"Content-Type": "application/json",
				apiKey: "TEST1236C4CF23E6921C41429A6E1D546AC9535E",
			  },
			  body: JSON.stringify(newData),
			});
  
			const responseData = await response.json();
			if (response.ok) {
			  // Update the grid with the newly added record
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
		  const updatedData = {
			// Provide the updated record data
			
			firstName: e.newData.firstName,
			surname: e.newData.surname,
			email: e.newData.email,
			mobile: e.newData.mobile,
		  };
  
		  try {
			const response = await fetch(`https://api.recruitly.io/api/candidate/${e.key}`, {
			  method: "POST",
			  headers: {
				"Content-Type": "application/json",
				apiKey: "TEST1236C4CF23E6921C41429A6E1D546AC9535E",
			  },
			  body: JSON.stringify(updatedData),
			});
  
			const responseData = await response.json();
			if (response.ok) {
			  // Update the grid with the updated record
			  const updatedItemIndex = gridData.findIndex((item) => item.id === e.key);
			  gridData[updatedItemIndex] = e.newData;
			  dataGrid.refresh();
			} else {
			  console.error("Failed to update record:", responseData.error);
			}
		  } catch (error) {
			console.error("Failed to update record:", error);
		  }
		},
		onRowRemoving: async (e) => {
		  try {
			const response = await fetch(`https://api.recruitly.io/api/candidate/${e.key}`, {
			  method: "DELETE",
			  headers: {
				apiKey: "TEST1236C4CF23E6921C41429A6E1D546AC9535E",
			  },
			});
  
			if (response.ok) {
			  // Remove the record from the grid
			  const removedItemIndex = gridData.findIndex((item) => item.id === e.key);
			  gridData.splice(removedItemIndex, 1);
			  dataGrid.refresh();
			} else {
			  console.error("Failed to delete record.");
			}
		  } catch (error) {
			console.error("Failed to delete record:", error);
		  }
		},
	  });
  
	  dataGrid.render();
	});
  </script>
  
  <style>
	/* Custom CSS for popups */
	.custom-popup .dx-popup-title {
	  background-color: #f0f0f0;
	  color: #333;
	  font-weight: bold;
	  padding: 10px;
	}
  
	.custom-popup .dx-popup-content {
	  display: flex;
	  flex-direction: column;
	  padding: 10px;
	}
  
	.custom-popup .dx-form .dx-item {
	  margin-bottom: 10px;
	}
  
	.custom-popup .dx-form .dx-field-item-label {
	  width: 100px;
	  text-align: right;
	  margin-right: 10px;
	  font-weight: bold;
	}
  
	.custom-popup .dx-form .dx-field-item-content {
	  flex-grow: 1;
	}
  
	.custom-popup .dx-button {
	  margin-top: 10px;
	}
	
 
  .custom-popup .dx-popup-title {
    background-color: #f0f0f0;
    color: #333;
    font-weight: bold;
    padding: 10px;
  }
  
  .custom-popup .dx-popup-content {
    display: flex;
    flex-direction: column;
    padding: 10px;
  }
  
  .custom-popup .dx-form .dx-item {
    margin-bottom: 10px;
  }
  
  .custom-popup .dx-form .dx-field-item-label {
    width: 100px;
    text-align: right;
    margin-right: 10px;
    font-weight: bold;
  }
  
  .custom-popup .dx-form .dx-field-item-content {
    flex-grow: 1;
  }
  
  .custom-popup .dx-button {
    margin-top: 10px;
  }
</style>

 
  
  <div id="dataGrid"></div>
  