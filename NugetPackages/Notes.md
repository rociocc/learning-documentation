- When adding a service reference in VS 2022, it's useful to disable private Nuget sources before adding the reference, and enabling them afterwards.
- Add a Nuget package locally in VS: Tools -> Nuget Package Manager -> Package Manager Settings: Pakcage Sources - New Source
    Name:     Pavilion-Ticketing-Contract@Local
    Source:   C:\Users\RocioCardenas\Documents\Projects\PavRepos\ticketing-svc-vrt-api\packages
  - Then Update
- Package Manager Settings: General : Clear All Nuget Storage
- Restart VS
