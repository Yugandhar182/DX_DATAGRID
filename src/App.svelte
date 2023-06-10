
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
		firstname: item.firstname,
		surname: item.surname,
		email: item.email,
		mobile: item.mobile,
	  }));
  
	  console.log(gridData, "griddata");
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(document.getElementById("dataGrid"), {
		dataSource: gridData,
		columns: [
		  { dataField: "id", caption: "ID" },
		  { dataField: "firstname", caption: "First Name" },
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
			title: "Edit Candidate",
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
	  });
  
	  dataGrid.render();
	});
  </script>
  
  <div id="dataGrid"></div>
  