# How to dynamically modify the hover color of rows in Flutter DataTable (SfDataGrid)?.

In this article, we will show you how to dynamically modify the hover color of rows in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required properties. You can change the row highlighting color by using the [SfDataGridThemeData.rowHoverColor](https://pub.dev/documentation/syncfusion_flutter_core/latest/theme/SfDataGridThemeData/rowHoverColor.html) property. By wrapping the cells with a [MouseRegion](https://api.flutter.dev/flutter/widgets/MouseRegion-class.html), you can detect mouse enter and exit events to change the hover color. Use `setState` to update the rowHoverColor and trigger a rebuild of the SfDataGrid to reflect the changes.

```dart
  @override
  void initState() {
    super.initState();
    employees = getEmployeeData();
    employeeDataSource = EmployeeDataSource(setState, employeeData: employees);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: SfDataGridTheme(
        data: SfDataGridThemeData(
            rowHoverColor: employeeDataSource._rowHoverColor),
        child: SfDataGrid(
          source: employeeDataSource,
          columnWidthMode: ColumnWidthMode.fill,
          columns: getColumn(),
        ),
      ),
    );
  }

class EmployeeDataSource extends DataGridSource {
  EmployeeDataSource(this.setState, {required List<Employee> employeeData}) {
   …..
  }

  List<DataGridRow> _employeeData = [];

  StateSetter setState;

  Color? _rowHoverColor;

 …..

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    MaterialColor getColorBasedOnRowIndex(DataGridRow dataGridRow) {
      int index = _employeeData.indexOf(dataGridRow);

      if (index % 2 == 0 || index == 0) {
        return Colors.amber;
      } else if (index == 1) {
        return Colors.purple;
      } else if (index == 3) {
        return Colors.pink;
      } else if (index == 5) {
        return Colors.teal;
      } else if (index == 7) {
        return Colors.cyan;
      } else if (index == 9) {
        return Colors.red;
      } else {
        return Colors.indigo;
      }
    }

    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((e) {
      return MouseRegion(
        onEnter: (event) {
          setState(() {
            _rowHoverColor = getColorBasedOnRowIndex(row);
          });
        },
        onExit: (event) {
          setState(() {
            _rowHoverColor = null;
          });
        },
        child: Container(
          alignment: Alignment.center,
          padding: const EdgeInsets.all(8.0),
          child: Text(e.value.toString()),
        ),
      );
    }).toList());
  }
}
```
You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-dynamically-modify-the-hover-color-of-rows-in-Flutter-DataTable).