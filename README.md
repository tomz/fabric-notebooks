# Portable Microsoft Fabric Spark notebooks

Runnable Microsoft Fabric editions of the Spark reference material:

| Notebook | Purpose |
|---|---|
| `spark_api_reference_fabric_runtime_2_0.ipynb` | Executable PySpark API reference |
| `spark_user_guide_fabric_runtime_2_0.ipynb` | Executable Spark user guide |
| `spark_tutorials_fabric_runtime_2_0.ipynb` | End-to-end Spark tutorials |
| `fabric_lakehouse_copy_move_all.ipynb` | Generic phased copy/move of Lakehouse tables, Files, shortcuts, and Spark views |

These notebooks target **Microsoft Fabric Runtime 2.0 / Apache Spark 4.1**. They use Fabric managed `spark` session and do not create, connect to, or stop a Spark session themselves.

## Use in any Fabric workspace

1. Download or clone this repository.
2. Import an `.ipynb` file into your Microsoft Fabric workspace.
3. Select a published **Fabric Runtime 2.0** environment, or configure Runtime 2.0 as your workspace default.
4. Attach any Lakehouse that you can write to as the notebook default Lakehouse.
5. Run all cells.

The notebooks contain no workspace, Lakehouse, environment, tenant, user, or deployment IDs. Their `metadata.dependencies.lakehouse` value is intentionally unbound so every user can select their own resources.

## Lakehouse copy/move workflow

`fabric_lakehouse_copy_move_all.ipynb` is runtime-neutral and uses Fabric
workspace and Lakehouse UUID parameters rather than display names. Run it in
three explicit phases:

1. Attach the source Lakehouse and run `copy` first as a dry run, then apply it.
2. Attach the target Lakehouse and run `finalize` with the full `manifestRoot`
   returned by copy.
3. For a move only, attach the source and run `delete-source` after validation
   with the exact source-bound confirmation token.

The notebook defaults to `dry_run = True`, supports pipeline-friendly text
parameters, and never deletes a Lakehouse item.

## Fabric Git integration

Each notebook has a matching `.platform.json` definition for Fabric Git integration. Keep each pair together:

```text
spark_api_reference_fabric_runtime_2_0.ipynb
spark_api_reference_fabric_runtime_2_0.platform.json
```

The platform definitions use schema version 2.0 and contain portable display metadata only.

## Runtime behavior

The notebooks:

- validate that the active Spark runtime is Apache Spark 4.1;
- print the active Fabric workspace, notebook, environment, Spark version, and application ID;
- preserve the breadth of the corresponding Spark 4.2 Playground source notebooks while adapting APIs and documentation to Fabric Runtime 2.0 / Spark 4.1;
- use the attached Lakehouse `Files/` area for write/read round trips;
- avoid Spark Connect-only APIs and session lifecycle calls.

## Validation

The three notebook editions were executed sequentially through Fabric RunNotebook under Runtime 2.0 after generation. Every deployed notebook completed successfully. The repository files themselves remain tenant-neutral; live deployment identifiers are deliberately not included.
