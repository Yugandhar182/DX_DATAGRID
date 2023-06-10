<script>

    import { onMount } from "svelte";

    // Sample data

    let jsonData = [];

    let data = [];

 

    onMount(async () => {

        const response = await fetch(

            "https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E"

        );

        const responseData = await response.json();

        jsonData = responseData.data;

        console.log(jsonData, "json");

        const gridData = jsonData.map(item => ({

   

   firstname:item.firstName,

      surname: item.surnameame,

      email:item.email,

      mobile:item.mobile,





      // map other properties accordingly

    }));

console.log(gridData,"griddata");

    // });

 

    var dataGrid = new DevExpress.ui.dxDataGrid("#dataGrid", {

        dataSource: gridData,

        columns: [

        { dataField: 'firstName', caption: 'firstName' },

        { dataField: 'surnname', caption: 'surname' },

        { dataField: 'email', caption: 'Email' },

        { dataField: 'mobile', caption: 'mobile' },

        // Define other columns as needed

      ],

 

        

        showBorders: true,

        filterRow: {

            visible: true,

        },

        // editing: {

            

        //  allowUpdating: true,

        //  allowDeleting: true,

        //  allowAdding: true,

        // },





        editing: {

            allowDeleting: true,

            allowAdding: true,

            allowUpdating: true,

            mode: "popup",

            form: {

                labelLocation: "top"

            },

            popup: {

                showTitle: true,

                title: "Editing candidate Details"

            }

        },

        paging: {

            pageSize: 5,

        },

        // pagination:true,

       

    });

    });

    // dataGrid.render();

    // });

</script>

<div id="dataGrid"></div>