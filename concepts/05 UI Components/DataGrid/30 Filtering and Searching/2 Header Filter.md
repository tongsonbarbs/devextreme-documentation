A header filter allows a user to filter values in an individual column by including or excluding them from the applied filter. Clicking a header filter icon invokes a popup menu with all the column's unique values. A user includes or excludes values from the filter by selecting or clearing their selection in this menu.

![DevExtreme HTML5 JavaScript jQuery Knockout Angular DataGrid Filtering HeaderFilter](/images/DataGrid/visual_elements/header_filter.png)

Assign **true** to the [headerFilter](/api-reference/10%20UI%20Components/GridBase/1%20Configuration/headerFilter '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/headerFilter/').**visible** property to make header filter icons visible for all columns. Set a column's [allowHeaderFiltering](/api-reference/_hidden/GridBaseColumn/allowHeaderFiltering.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/#allowHeaderFiltering') property to **false** if its header filter should not be available. Note that this property inherits the [allowFiltering](/api-reference/_hidden/dxDataGridColumn/allowFiltering.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/#allowFiltering') property's value by default.

---
##### jQuery

    <!--JavaScript-->$(function() {
        $("#dataGridContainer").dxDataGrid({
            // ...
            headerFilter: { visible: true },
            columns: [{
                // ...
                allowHeaderFiltering: false
            }]
        });
    });

##### Angular
    
    <!--HTML-->
    <dx-data-grid ... >
        <dxo-header-filter [visible]="true"></dxo-header-filter>
        <dxi-column [allowHeaderFiltering]="false" ... ></dxi-column>
    </dx-data-grid>

    <!--TypeScript-->
    import { DxDataGridModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        // ...
    }
    @NgModule({
        imports: [
            // ...
            DxDataGridModule
        ],
        // ...
    })

##### Vue

    <!-- tab: App.vue -->
    <template>
        <DxDataGrid ... >
           <DxHeaderFilter :visible="true" />
           <DxColumn :allow-header-filtering="false" ... />
        </DxDataGrid>
    </template>

    <script>
    import 'devextreme/dist/css/dx.light.css';

    import DxDataGrid, {
        DxColumn,
        DxHeaderFilter
    } from 'devextreme-vue/data-grid';

    export default {
        components: {
            DxDataGrid,
            DxColumn,
            DxHeaderFilter
        }
    }
    </script>

##### React

    <!-- tab: App.js -->
    import React from 'react';
    import 'devextreme/dist/css/dx.light.css';

    import DataGrid, {
        Column,
        HeaderFilter
    } from 'devextreme-react/data-grid';

    function App() {
        return (
            <DataGrid ... >
                <HeaderFilter visible={true} />
                <Column allowHeaderFiltering={false} ... />
            </DataGrid>
        );
    }

    export default App;

##### ASP.NET MVC Controls

    <!--Razor C#-->
    @(Html.DevExtreme().DataGrid()
        @* ... *@
        .HeaderFilter(hf => hf.Visible(true))
        .Columns(columns => {
            columns.Add().AllowHeaderFiltering(false);
        })
    )

---

A user can change the applied filter by including or excluding values. Use a column's [filterType](/api-reference/_hidden/GridBaseColumn/filterType.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/#filterType') property to specify the required mode. You can specify the initial filter by combining this property and the [filterValues](/api-reference/_hidden/GridBaseColumn/filterValues.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/#filterValues') property. To change it at runtime, call the [columnOption](/api-reference/10%20UI%20Components/GridBase/3%20Methods/columnOption(id_options).md '/Documentation/ApiReference/UI_Components/dxDataGrid/Methods/#columnOptionid_options') method:

---
##### jQuery

    <!--JavaScript-->$(function() {
        $("#dataGridContainer").dxDataGrid({
            // ...
            columns: [{
                // ...
                dataField: "OrderDate",
                filterType: "exclude", // or "include"
                filterValues: [2014]
            }]
        });
    });

<!---->

    <!--JavaScript-->
    $("#dataGridContainer").dxDataGrid("instance").columnOption("OrderDate", {
        filterType: "include",
        filterValues: [2014, 2015]
    });

##### Angular
    
    <!--HTML-->
    <dx-data-grid ... >
        <dxi-column 
            dataField="OrderDate"
            [(filterValues)]="filterValues"
            [(filterType)]="filterType"> 
        </dxi-column>
    </dx-data-grid>

    <!--TypeScript-->
    import { DxDataGridModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        filterValues: Array<any> = [2014];
        filterType: string = "exclude";    // or "include"
        applyFilter (filterType, values) {
            this.filterType = filterType;
            this.filterValues = values;
        }
    }
    @NgModule({
        imports: [
            // ...
            DxDataGridModule
        ],
        // ...
    })

##### Vue

    <!-- tab: App.vue -->
    <template>
        <DxDataGrid ... >           
            <DxColumn 
                v-model:filter-type="filterType"
                v-model:filter-values="filterValues" 
                data-field="OrderDate"
            />
        </DxDataGrid>
    </template>

    <script>
    import 'devextreme/dist/css/dx.light.css';

    import DxDataGrid, {
        DxColumn
    } from 'devextreme-vue/data-grid';

    export default {
        components: {
            DxDataGrid,
            DxColumn
        },
        data() {
            return {
               filterType: "exclude", // or "include" 
               filterValues: [2014]
            }
        },
        methods: {
            applyFilter (filterType, values) {
                this.filterType = filterType;
                this.filterValues = values;
            }
        }
    }
    </script>

##### React

    <!-- tab: App.js -->
    import React, { useState, useCallback } from 'react';
    import 'devextreme/dist/css/dx.light.css';

    import DataGrid, {
        Column
    } from 'devextreme-react/data-grid';

    function App() {
        const [filterType, setFilterType] = useState('exclude'); // or 'include'
        const [filterValues, setFilterValues] = useState([2014]);

        const onOptionChanged = useCallback((e) => {
            if (e.fullName === "columns[0].filterValues") {
                setFilterValues(e.value);
            }
            if (e.fullName === "columns[0].filterType") {
                setFilterType(e.value);
            }
        }, []);

        const applyFilter = (filterType, values) => {
            setFilterType(filterType);
            setFilterValues(values);
        };

        return (
            <DataGrid onOptionChanged={onOptionChanged} ... >
                <Column 
                    dataField="OrderDate"
                    filterType={filterType}                   
                    filterValues={filterValues}
                />
            </DataGrid>
        );
    }

    export default App;

##### ASP.NET MVC Controls

    <!--Razor C#-->
    @(Html.DevExtreme().DataGrid()
        @* ... *@
        .ID("dataGridContainer")
        .Columns(columns => {
            columns.Add()
                .DataField("OrderDate")
                .FilterType(FilterType.Exclude) // or FilterType.Include
                .FilterValues(new int[] { 2014 });
        })
    )

    <script type="text/javascript">        
        $("#dataGridContainer").dxDataGrid("instance").columnOption("OrderDate", {
            filterType: "include",
            filterValues: [2014, 2015]
        });
    </script>


---

#include datagrid-filtering-rowandheaderconflicts

You can use the **headerFilter.**[allowSearch](/api-reference/10%20UI%20Components/GridBase/1%20Configuration/headerFilter/allowSearch.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/headerFilter/#allowSearch') property to enable searching in the header filter. You can also declare this property in a column's configuration object to enable/disable searching in this column's header filter.

---
##### jQuery

    <!--JavaScript-->
    $(function() {
        $("#dataGridContainer").dxDataGrid({
            // ...
            headerFilter: { 
                visible: true,
                allowSearch: true
            },
            columns: [{
                // ...
                headerFilter: { 
                    allowSearch: false
                }
            }]
        });
    });

##### Angular
    
    <!--HTML-->
    <dx-data-grid ... >
        <dxo-header-filter [visible]="true" [allowSearch]="true"></dxo-header-filter>
        <dxi-column ... >
            <dxo-header-filter [allowSearch]="false"></dxo-header-filter>
        </dxi-column>
    </dx-data-grid>

    <!--TypeScript-->
    import { DxDataGridModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        // ...
    }
    @NgModule({
        imports: [
            // ...
            DxDataGridModule
        ],
        // ...
    })
##### Vue

    <!-- tab: App.vue -->
    <template>
        <DxDataGrid ... >
            <DxHeaderFilter 
                :allow-search="true" 
                :visible="true" 
            />
            <DxColumn>
                <DxColumnHeaderFilter :allow-search="false" />
            </DxColumn>
        </DxDataGrid>
    </template>

    <script>
    import 'devextreme/dist/css/dx.light.css';

    import DxDataGrid, {
        DxColumn,
        DxHeaderFilter,
        DxColumnHeaderFilter
    } from 'devextreme-vue/data-grid';

    export default {
        components: {
            DxDataGrid,
            DxColumn,
            DxHeaderFilter,
            DxColumnHeaderFilter
        }
    }
    </script>

##### React

    <!-- tab: App.js -->
    import React from 'react';
    import 'devextreme/dist/css/dx.light.css';

    import DataGrid, {
        Column,
        HeaderFilter,
        ColumnHeaderFilter
    } from 'devextreme-react/data-grid';

    function App() {
        return (
            <DataGrid ... >
                <HeaderFilter 
                    allowSearch={true} 
                    visible={true} 
                />
                <Column>
                    <ColumnHeaderFilter allowSearch={false} />
                </Column>
            </DataGrid>
        );
    }

    export default App;

##### ASP.NET MVC Controls

    <!--Razor C#-->
    @(Html.DevExtreme().DataGrid()
        @* ... *@
        .HeaderFilter(hf => hf
            .Visible(true)
            .AllowSearch(true)
        )
        .Columns(columns => {
            columns.Add()
                .HeaderFilter(hf => hf.AllowSearch(false));
        })
    )    

---

A header filter's popup menu lists all column values by default. You can group them using the **headerFilter**.[groupInterval](/api-reference/_hidden/GridBaseColumn/headerFilter/groupInterval.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/headerFilter/#groupInterval') property if they are numbers or dates. You can also provide a custom data source for a header filter using the [dataSource](/api-reference/_hidden/GridBaseColumn/headerFilter/dataSource.md '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/columns/headerFilter/#dataSource') property. Refer to the property's description for details.

#####See Also#####
- [Filtering API - Initial and Runtime Filtering](/concepts/05%20UI%20Components/DataGrid/30%20Filtering%20and%20Searching/6%20API/1%20Initial%20and%20Runtime%20Filtering.md '/Documentation/Guide/UI_Components/DataGrid/Filtering_and_Searching/#API/Initial_and_Runtime_Filtering')
- [remoteOperations](/api-reference/10%20UI%20Components/dxDataGrid/1%20Configuration/remoteOperations '/Documentation/ApiReference/UI_Components/dxDataGrid/Configuration/remoteOperations/')
- [DataGrid Demos](https://js.devexpress.com/Demos/WidgetsGallery/Demo/DataGrid/Filtering)
