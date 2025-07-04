To add breaking changes and preview messages for cmdlets, there are two scenarios to handle.

- For auto gen cmdlets
- For customized cmdlets

# For auto gen cmdlets

Breaking changes and preview messages for auto gen cmdlets are added through directives in readme.md. And following are some common cases.

## Case 1 — Module is deprecated

 

```yaml
- where:
    verb: (.*)
  set:
    breaking-change:
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2055/10/30
```

## Case 2 — Breaking change for a cmdlet

```yaml
- where:
    verb: New
    subject: VNetPeering
  set:
    breaking-change:
      replacement-cmdlet: New-AzNewVNetPeering
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2022/05/30
```

## Case 3 — Breaking change for multiple cmdlets

```yaml
- where:
    subject: VNetPeering
  set:
    breaking-change:
      replacement-cmdlet: $.replace("VNetPeering", "VNewNetPeering")
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2022/05/30
```

## Case 4 — Breaking change for an output type

```yaml
- where:
    verb: New
    subject: VNetPeering
  set:
    breaking-change:
      deprecated-cmdlet-output-type: oldtype
      replacement-cmdlet-output-type: newtype
      deprecated-output-properties:
        - propertyA
        - PropertyB
      new-output-properties:
        - PropertyC
        - PropertyD
      change-description: This is a custom message for the change.
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2022/05/11
```

## Case 5 — Breaking change for parameter sets(variants)

```yaml
- where:
    verb: Remove
    subject: VNetPeering
    variant: Delete
  set:
    breaking-change:
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2022/05/30
```

## Case 6 — Breaking change for a parameter

```yaml
- where:
    parameter-name: Sku
  set:
    breaking-change:
      old-parameter-type: int
      new-parameter-type: boolean
      become-mandatory: true
      change-description: This is a custom message for the change.
      deprecated-by-version: 5.0.0
      deprecated-by-azversion: 20.0.0
      change-effective-date: 2022/05/30
```

## Case 7 — Preview message

```yaml
- where:
    verb: New
    subject: VNetPeering
  set:
    preview-announcement:
      preview-message: This is a test preview message.
      estimated-ga-date: 2023-09-30
```

# For customized cmdlets

To add breaking changes or preview messages for a customized cmdlets, you will need to add related attributes in code directly. And following are some common cases.

You must provide expected breaking change az version and module version otherwise it won't compile. The first version is expected az version while the second one is expected module version.

**Note: these examples are based on the Az.Databricks module. Please double check the namespace. You will most likely need to replace "Databricks" with your module's name.**

## Case 1 — Generic Breaking change for a cmdlet

```csharp
[Microsoft.Azure.PowerShell.Cmdlets.Databricks.Runtime.GenericBreakingChangeAttribute("message about the change", "16.0.0", "4.0.0", "2022/05/30")
```

## Case 2 — Breaking change for a cmdlet

```csharp
[Microsoft.Azure.PowerShell.Cmdlets.Databricks.Runtime.CmdletBreakingChangeAttribute("16.0.0", "4.0.0", "2022/05/30", ReplacementCmdletName = 'replace-xxx')
```

## Case 3 — Breaking change for an output type

```csharp
[Microsoft.Azure.PowerShell.Cmdlets.Databricks.Runtime.OutputBreakingChangeAttribute("oldtype", "11.0.0", "5.0.0", "2022/05/11", ReplacementCmdletOutputType = "newtype", DeprecatedOutputProperties = ("propertyA", "PropertyB"), NewOutputProperties = ("PropertyC", "PropertyD"))]
```

## Case 4 — Breaking change for a parameter

```csharp
[Microsoft.Azure.PowerShell.Cmdlets.Databricks.Runtime.ParameterBreakingChangeAttribute("ResourceGroupName", "11.0.0", "4.1.0", "2028/06/18")]
```

## Case 5 — Preview message

```csharp
[Microsoft.Azure.PowerShell.Cmdlets.Databricks.Runtime.PreviewMessageAttribute("This is a preview version", "2028/06/18")]
```
