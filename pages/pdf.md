---
layout: default
date: 2020-02-19
---

# About pdf files in Revit

## Exporting pdfs

### Pdf file size

**Most important: vector or raster pdf**

Raster pdfs file size doesn't depend on the number of displayed elements, only on the resolution.

Vector pdfs file size can grow huge if you display a lot of elements, or if you use heavy hatches. However as they are vectors, they don't have a resolution, you can zoom in indefinitely (or until the pdf viewer lets you zoom in) without losing a detail. Also view scale doesn't matter with vector printing, 1:50 and 1:100 should generate similar file size.

Revit will show you a warning if you select vector, but it can only print in raster. The reasons for this are in the help: [Revit help](https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Revit-DocumentPresent/files/GUID-EDEA2E5D-6094-4B9D-A4E7-43C95A3E0615-htm.html)

Which one is the better depends on the use case of the pdfs:

- If you will just print them, display on a monitor doesn't matter, than check the resolution of the printer and print a raster pdf in that resolution.
- If you want to view it on a screen, it's better to stay on vector. Try to repace your hatches to something not that heavy, a lot of hatches contain unnecessary lines. Also check if some families has a lot of lines which are hidden by lineweight. 

*This note was based on my comment on Revitforum: [Link](https://revitforum.org/showthread.php/42794-PDF-File-Size?p=228499#post228499)*
