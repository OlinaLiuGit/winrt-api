---
-api-id: M:Windows.ApplicationModel.AppExtensions.AppExtensionCatalog.RequestRemovePackageAsync(System.String)
-api-type: winrt method
---

<!-- Method syntax
public Windows.Foundation.IAsyncOperation<bool> RequestRemovePackageAsync(System.String packageFullName)
-->

# Windows.ApplicationModel.AppExtensions.AppExtensionCatalog.RequestRemovePackageAsync

## -description
Removes the specified extension package.

## -parameters
### -param packageFullName
The name of the package to remove.
<!--how do you know the 'full' name?-->

## -returns
Returns **true** if the package was successfully removed; otherwise, **false**.

## -remarks
The user is prompted to allow or deny the removal of the package.

[Desktop Bridge](https://developer.microsoft.com/en-us/windows/bridges/desktop) app extension hosts cannot use this method directly. Desktop Bridge app extension hosts should use their Universal Windows Platform component to manage app extensions.

## -examples

## -see-also
