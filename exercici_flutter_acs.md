---
marp: true
theme: default
size: 16:9
paginate: true
---

Exercise Flutter : 
User Groups in ACS app
===

![bg right:35% height:700](group.png)

---

The problem
===

Program this: https://github.com/disseny-de-software/exercise_flutter_acs_slides

---

Program in Flutter the user interface of the ACS app to manage the groups of users :

1. list existing groups
1. for a group show *and edit*
    1. name and description
    1. schedule = from and to dates and times, weekdays
    1. allowed actions
    1. list of users
    1. *not* the allowed areas for the moment
1. show user data (name + credential) and edit them
1. add a new group (but not delete)
1. add a new user of a group (but not delete)

---

8 Screens + drawer
===

![width:250](drawer.png)  ![width:250](list_user_groups.png) ![width:250](group.png) ![width:250](info.png)

---

![width:250](schedule.png)  ![width:250](actions.png) ![width:250](list_users.png) ![width:250](user.png)

---

Code provided
===

[``data.dart``](https://github.com/disseny-de-software/exercise_flutter_acs/src/data.dart)
 
The data of the admin, managers and employees is in Flutter, no webserver here.

- data are stored in objects of the corresponding classes: ``Group`` class with a ``Schedule``, a list of allowed ``Action`` and list of ``User``


- all data are in a ``Data`` class, in ``static`` attributes so that they are a kind of global variables you can access just importing ``data.dart``

- plus the default values to be shown in the interface when creating a new user group.

---

[``main.dart``](https://github.com/disseny-de-software/exercise_flutter_acs/src/main.dart)

The entry point of the app. As always, sets the ``ThemeData`` which means the ``ColorScheme`` and ``TextTheme``.

``ColorScheme`` means a color palette chosen around a main color (here ``Colors.blue``), defining a primary/secondary/tertiary color, backgrounds, colors on top of primary etc. This determines the colors of the app bar, buttons, text etc.

```dart
appBar: AppBar(
  backgroundColor: Theme.of(context).colorScheme.primary, // blue
  foregroundColor: Theme.of(context).colorScheme.onPrimary, // white
  title: Text("ACS"),
),
```

![](appbar.png) ![height:60](button.png) ![height:60](icon.png) ![height:60](fab.png) 

Read this https://docs.flutter.dev/cookbook/design/themes

---

![bg right:35% height:700](blank.png)

[``screen_blank.dart``](https://github.com/disseny-de-software/exercise_flutter_acs/src/screen_blank.dart)

- No content, first screen shown when executing.
- Just want it to open the drawer and go to list of groups
- Scaffold ( Drawer, AppBar ), no ``body``

 
---

![bg right:35% height:700](drawer.png)

[``drawer.dart``](https://github.com/disseny-de-software/exercise_flutter_acs/src/screen_blank.dart)

Defines class ``TheDrawer`` with attribute ``drawer``,  an instance of Flutter ``Drawer``. This is because several screens have this same drawer.

Next screen :
```dart
onTap: () {
    Navigator.of(context).pop(); // close drawer
    Navigator.of(context).push(MaterialPageRoute<void>(
      builder: (context) => 
        ScreenListGroups(userGroups: Data.userGroups),
    )); // go to screen of list of groups

},
```
 
Guidelines to make a drawer https://flutter.dev/docs/cookbook/design/drawer

---

![bg right:35% height:700](list_user_groups.png)


[``screen_list_groups.dart``](https://github.com/disseny-de-software/exercise_flutter_acs/src/screen_list_groups.dart)

- ``ScreenListGroups`` receives ``userGroups`` from ``TheDrawer``:
  ```dart
  class ScreenListGroups extends StatefulWidget {
    List<UserGroup> userGroups;

    ScreenListGroups({super.key, 
      required this.userGroups});
  ```
  
- so that its state can have access to it to show its names and number of users:
  ```dart
  class _ScreenListGroupsState 
    extends State<ScreenListGroups> {
    late List<UserGroup> userGroups;

    @override
    void initState() {
      super.initState();
      userGroups = widget.userGroups; // of ScreenListGroups
    }
  ```


---

TODO
===

We provide you the former 5 files, you don't need to change them except for ``screen_list_groups.dart`` where you have to implement ``onTap()`` of ``_buildRow()`` in order to go to the group screen, to start showing the $i$-th group in the list.

---

Hint:

```dart
Widget _buildRow(UserGroup userGroup, int index) {
  return ListTile(
      title: Text(userGroup.name),
      trailing: Text('${userGroup.users.length}'),
      onTap: () => Navigator.of(context)
          .push(MaterialPageRoute<void>(
                  builder: (context) => ScreenGroup(userGroup: userGroup)),
                )
          .then((var v) => setState(() {})),
  );
}
```

Why ``.then`` ? When we come back from ``ScreenGroup`` where maybe we have changed the group name or added users, we want to show the new name and present number of users.


---

![bg right:35% height:700](group.png)

```dart
Scaffold
  appBar: AppBar
  body: Center(
    child: Gridview.count(
      children: <Widget>[
        Container(
          child: Center(
            child: Column(
              children: <Widget>[
                IconButton(icon: Icon),
                Text,]
```

See [IconButton](https://api.flutter.dev/flutter/material/IconButton-class.html), [Column](https://api.flutter.dev/flutter/widgets/Column-class.html), [Gridview](https://api.flutter.dev/flutter/material/Card-class.html) and Material's [catalog of icons](https://material.io/resources/icons) (can be different)

---

![bg right:35% height:700](info.png)

```dart
Scaffold(
  appBar: AppBar
  body: Form(
    child: Container(
      child: Column(
        children: <Widget>[
          TextFormField,
          TextFormField,
          Padding(
            child: ElevatedButton(
              child: Text
```

See ``Form`` [validation](https://docs.flutter.dev/cookbook/forms/validation) and how to [retrieve](https://docs.flutter.dev/cookbook/forms/retrieve-input) the value of a text field

To show a "saved" message after submit:

```dart
ScaffoldMessenger.of(context)
  .showSnackBar(
    const SnackBar(content: Text('Saved')),
);
```

---

![bg right:35% height:700](schedule.png)

---

![bg right:35% height:700](actions.png)

---

![bg right:35% height:700](list_users.png)

---

![bg right:35% height:700](user.png)

---