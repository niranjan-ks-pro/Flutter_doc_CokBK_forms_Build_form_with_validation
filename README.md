# Flutter_doc_CokBK_forms_Build_form_with_validation
 https://docs.flutter.dev/cookbook/forms/validation
Build a form with validation
============================

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Forms](https://docs.flutter.dev/cookbook/forms)
3.  [Build a form with validation](https://docs.flutter.dev/cookbook/forms/validation)

Apps often require users to enter information into a text field. For example, you might require users to log in with an email address and password combination.

To make apps secure and easy to use, check whether the information the user has provided is valid. If the user has correctly filled out the form, process the information. If the user submits incorrect information, display a friendly error message letting them know what went wrong.

In this example, learn how to add validation to a form that has a single text field using the following steps:

1.  Create a `Form` with a `GlobalKey`.
2.  Add a `TextFormField` with validation logic.
3.  Create a button to validate and submit the form.

[](https://docs.flutter.dev/cookbook/forms/validation#1-create-a-form-with-a-globalkey)1\. Create a `Form` with a `GlobalKey`
-----------------------------------------------------------------------------------------------------------------------------

First, create a [`Form`](https://api.flutter.dev/flutter/widgets/Form-class.html). The `Form` widget acts as a container for grouping and validating multiple form fields.

When creating the form, provide a [`GlobalKey`](https://api.flutter.dev/flutter/widgets/GlobalKey-class.html). This uniquely identifies the `Form`, and allows validation of the form in a later step.

content_copy

```
import 'package:flutter/material.dart';

// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  MyCustomFormState createState() {
    return MyCustomFormState();
  }
}

// Define a corresponding State class.
// This class holds data related to the form.
class MyCustomFormState extends State<MyCustomForm> {
  // Create a global key that uniquely identifies the Form widget
  // and allows validation of the form.
  //
  // Note: This is a `GlobalKey<FormState>`,
  // not a GlobalKey<MyCustomFormState>.
  final _formKey = GlobalKey<FormState>();

  @override
  Widget build(BuildContext context) {
    // Build a Form widget using the _formKey created above.
    return Form(
      key: _formKey,
      child: const Column(
        children: <Widget>[
          // Add TextFormFields and ElevatedButton here.
        ],
      ),
    );
  }
}
```

tips_and_updates Tip: Using a `GlobalKey` is the recommended way to access a form. However, if you have a more complex widget tree, you can use the [`Form.of()`](https://api.flutter.dev/flutter/widgets/Form/of.html) method to access the form within nested widgets.

[](https://docs.flutter.dev/cookbook/forms/validation#2-add-a-textformfield-with-validation-logic)2\. Add a `TextFormField` with validation logic
-------------------------------------------------------------------------------------------------------------------------------------------------

Although the `Form` is in place, it doesn't have a way for users to enter text. That's the job of a [`TextFormField`](https://api.flutter.dev/flutter/material/TextFormField-class.html). The `TextFormField` widget renders a material design text field and can display validation errors when they occur.

Validate the input by providing a `validator()` function to the `TextFormField`. If the user's input isn't valid, the `validator` function returns a `String` containing an error message. If there are no errors, the validator must return null.

For this example, create a `validator` that ensures the `TextFormField` isn't empty. If it is empty, return a friendly error message.

content_copy

```
TextFormField(
  // The validator receives the text that the user has entered.
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'Please enter some text';
    }
    return null;
  },
),
```

[](https://docs.flutter.dev/cookbook/forms/validation#3-create-a-button-to-validate-and-submit-the-form)3\. Create a button to validate and submit the form
-----------------------------------------------------------------------------------------------------------------------------------------------------------

Now that you have a form with a text field, provide a button that the user can tap to submit the information.

When the user attempts to submit the form, check if the form is valid. If it is, display a success message. If it isn't (the text field has no content) display the error message.

content_copy

```
ElevatedButton(
  onPressed: () {
    // Validate returns true if the form is valid, or false otherwise.
    if (_formKey.currentState!.validate()) {
      // If the form is valid, display a snackbar. In the real world,
      // you'd often call a server or save the information in a database.
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Processing Data')),
      );
    }
  },
  child: const Text('Submit'),
),
```
