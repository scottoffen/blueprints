# Strong Naming

Strong naming gives an assembly a unique identity by signing it with a cryptographic key pair. It is required for assemblies that need to be installed in the Global Assembly Cache (GAC), and is a common practice for public NuGet packages to ensure consumers can verify they are referencing the correct binary.

If strong naming is not a requirement for your project, remove the `StrongNaming` property group from `src/Directory.Build.props` and any references to `$(MyPackagePublicKey)` in project files across the repository. No further steps are needed.

## Generating the Key Pair

Strong name key generation requires `sn.exe`, which is a Windows-only tool. Generate the `.snk` file once on Windows, commit it to the repository, and all platforms can use it from there.

Open the **Developer Command Prompt for Visual Studio** (search for it in the Start menu), which puts `sn` on your PATH automatically. Navigate to the `src` folder of your repository, then run:

```cmd
sn -k MyPackage.snk
```

---

## Extracting the Public Key

From the same `src` folder, extract the public key and print it to the console:

```cmd
sn -p MyPackage.snk MyPackage.PublicKey.snk
sn -tp MyPackage.PublicKey.snk
```

This prints output like this:

```
Public key (hash algorithm: sha1):
0024000004800000...long hex string...

Public key token is 1a2b3c4d5e6f7a8b
```

Copy the full public key -- the long hex string, not the short token at the bottom. Then delete the intermediate file, which is no longer needed:

```cmd
del MyPackage.PublicKey.snk
```

---

## Configuring Directory.Build.props

The `src/Directory.Build.props` file already has a `StrongNaming` property group. Paste the full public key into the `MyPackagePublicKey` property:

```xml
<PropertyGroup Label="StrongNaming">
  <SignAssembly>true</SignAssembly>
  <AssemblyOriginatorKeyFile>$(MSBuildThisFileDirectory)MyPackage.snk</AssemblyOriginatorKeyFile>
  <PublicSign Condition="'$(OS)' != 'Windows_NT'">true</PublicSign>
  <MyPackagePublicKey>0024000004800000...</MyPackagePublicKey>
</PropertyGroup>
```

Because this lives in `Directory.Build.props`, the `$(MyPackagePublicKey)` property is available to every project file in the repository, including `InternalsVisibleTo` declarations:

```xml
<ItemGroup>
  <InternalsVisibleTo Include="MyPackage.Tests, PublicKey=$(MyPackagePublicKey)" />
</ItemGroup>
```

---

## The `.snk` File

The `.snk` file contains both the private and public key. It is safe to commit to a public repository -- strong name keys are not a security mechanism for protecting code, they are an identity mechanism for ensuring assembly compatibility. Contributors on macOS and Linux do not need to generate or interact with the key file directly; the `PublicSign` property handles builds on those platforms automatically.