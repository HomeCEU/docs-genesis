# Course Lookup

## Actors
1. cems admins

## Flow
1. Login
1. Open "Course Lookup"
1. Find course by id or name
1. See Course Detail
    - id
    - name
    - created_on
    - hours
    - Format
    - Disciplines
    - Authors
    - Categories
    - Clinical Topics
    - Owner
    - isActive
    - Instructional Level
    - BOC Instructional Level
    - Total Enrollments
    - Sco filename



## Flowchart
[![](https://mermaid.ink/img/eyJjb2RlIjoiZ3JhcGggVEQgIFxuICBMKExvZ2luIGFzIENFTVMgQWRtaW4pLS0-XG4gIENMKE9wZW4gQ291cnNlIExvb2t1cCktLT5cbiAgRihGaW5kIGNvdXJzZSBieSBpZCBvciBuYW1lKVxuICBGLS0-Q0QoU2VlIENvdXJzZSBEZXRhaWwpXG4gIEYtLT5Be1ByZWZvcm0gQWN0aW9ufVxuICBBLS0-fENvcHkgQ291cnNlfEMoTG9hZCBuZXcgY291cnNlKVxuICBBLS0-fEFkZCBTQ098IEFkZFNjbyhTZWxlY3QgU0NPIEZpbGUpLS0-UyhTdWJtaXQpXG4gIEEtLT58R2VuZXJhdGUgUHJveHkgRmlsZXN8IFNlbENvKFNlbGVjdCBDb21wYW55KS0tPlMyKFN1Ym1pdClcblxuIiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVEQgIFxuICBMKExvZ2luIGFzIENFTVMgQWRtaW4pLS0-XG4gIENMKE9wZW4gQ291cnNlIExvb2t1cCktLT5cbiAgRihGaW5kIGNvdXJzZSBieSBpZCBvciBuYW1lKVxuICBGLS0-Q0QoU2VlIENvdXJzZSBEZXRhaWwpXG4gIEYtLT5Be1ByZWZvcm0gQWN0aW9ufVxuICBBLS0-fENvcHkgQ291cnNlfEMoTG9hZCBuZXcgY291cnNlKVxuICBBLS0-fEFkZCBTQ098IEFkZFNjbyhTZWxlY3QgU0NPIEZpbGUpLS0-UyhTdWJtaXQpXG4gIEEtLT58R2VuZXJhdGUgUHJveHkgRmlsZXN8IFNlbENvKFNlbGVjdCBDb21wYW55KS0tPlMyKFN1Ym1pdClcblxuIiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)

### Flowchart mermade-js source
```
graph TD  
  L(Login as CEMS Admin)-->
  CL(Open Course Lookup)-->
  F(Find course by id or name)
  F-->CD(See Course Detail)
  F-->A{Preform Action}
  A-->|Copy Course|C(Load new course)
  A-->|Add SCO| AddSco(Select SCO File)-->S(Submit)
  A-->|Generate Proxy Files| SelCo(Select Company)-->S2(Submit)

```
