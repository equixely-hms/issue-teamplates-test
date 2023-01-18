# issue-teamplates-test

the issue template can be found here:

- [bug_report.md](/.github/ISSUE_TEMPLATE/bug_report.md)

- [new_feature.md](/.github/ISSUE_TEMPLATE/new_feature.md)

# Go engine report

## Static constructors ⚠️

### Summary

change static constructors from containing the struct name to only be `New`

### Examples
```diff
package audit

+func New() (*Audit, error){...}
-func NewAudit() (*Audit, error){...}
```

```diff
package audit

-func NewRepository() (db *gorm.DB) *Repository{}
+func New() (db *gorm.DB) *Repository{}
```

```diff
package audit

-func NewUseCase() (db *gorm.DB) *UseCase{}
+func New() (db *gorm.DB) *Repository{}
```

### Sources
- https://go.dev/doc/effective_go
- https://pkg.go.dev/errors

inside "https://go.dev/doc/effective_go" you can find:

>  Similarly, the function to make new instances of ring.Ring—which is the definition of a constructor in Go—would normally be called NewRing, but since Ring is the only type exported by the package, and since the package is called ring, it's called just New, which clients of the package see as ring.New. Use the package structure to help you choose good names.

### Benefits

- forces to choose good names for the namespaces (ex: `auditRepo` for the repositories)

- removes redundancy, in the src code there is always this pattern:

    ```go
    audituc.NewUseCase(...)
    //or
    auditrepo.NewRepository(...)
    ```

    If `audituc` is not explicit enough I would suggest to change it to `auditUseCase`, the same goes for `auditrepo`.

    The result would be:

    ```go
    auditUseCase.New(...)
    //or
    auditRepositoy.New(...)
    ```

## UseCases and Repositories structs ⚠️

### Summary

Naming `Repository` each  `Repository` structs gives no insights about the type of repo and the cost of explicitly insert the `entity` name inside it is zero.

### Example

```diff
package audit

-type Repository struct {
+type AuditRepository struct {
	db *gorm.DB
}

```

### Sources
- https://github.com/evrone/go-clean-template

### Benefits

Naming each repos struct `Repository` gives no information about the repository to the client.

ex:

```go
a := auditRepo.New()
b := bookRepo.New()

// l and b type can be confused in the long run
```
## Project Layout

### Sources
- https://github.com/evrone/go-clean-template
- https://github.com/golang-standards/project-layout


### Use standard Go Project Layout ⚠️

- `📁repo`, `📁entities` and  `📁usecase` shold probably be placed in a single folder (`📁internal`).
- maybe `📁cmd/go-engine-equixely/cmd` in a top level `📁init` folder
- maybe `📁infratructure` e `📁utils` in a `📁pkg` folder 

### Renaming ⚠️

`📁entities` should be renamed `📁entity`to follow the `go-clean-template` naming scheme

## `📃repository/audit/common_test`

I'm confused about the need for this file, but i thing is my fault
