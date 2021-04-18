# version control

## Theoretical Knowledge

Version Control Systems (VCSs) are a group of software tools that help a development team manage changes to the source code over time. They allow the team to track every modification to the code, revert back to a previous version, compare changes, and recover lost files, all with very little overhead.

- Local VCS
- Centralized VCS
- Distributed VCS

### local VCS

- A local VCS has a simple database that keeps track of all changes to the project on the local system.
- It works by keeping the patch sets (changes/differences between the files) in a pre-defined format on disk.
- To re-create a file at any given point of time, it simply adds up all the patches.
- [Revision control system](https://en.wikipedia.org/wiki/Revision_Control_System)

### centralized VCS

- Centralized VCSs have a **single** VCS server where all the versioned files are stored.
- The users can check out any of those files at any time, make necessary changes, and upload them back to the server.
- Admins in the centralized VCS have more control over the clients and their access to the files.
- The obvious downside of this is the single point of failure. If the server suffers a blackout or a corrupted hard drive, data can be lost. Examples of centralized VCSs include CVS and Subversion.

### Distributed VCS

- Distributed VCSs do not just check out the latest versions or snapshots of the files, they also **mirror the entire repository** (repo). This includes its **full history**, with **changes** made by other users, **comparisons** between versions, etc. 
- In the case of a blackout or a corrupted disk on any server, the systems that were mirroring that server can be used to **restore** the repositories. 
- Each time the user clones a repo from the server, a **full backup** of all the data related to that repo gets cloned to the local system. 
- Examples of distributed VCSs include Git, Darcs and Bazaar;