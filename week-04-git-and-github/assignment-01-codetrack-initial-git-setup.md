# Assignment 1 — CodeTrack: Initial Git Setup (Local Only)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will set up Git correctly on your local machine before starting the CodeTrack project. You will create a local repository and configure your Git identity at both the repository level (local) and the machine level (global). This assignment is local only — you will not push anything to GitHub yet.

---

# Task 1 — Create the CodeTrack Project and Initialize Git

## Goal

Create a `CodeTrack` project folder and initialize it as a Git repository.

### Evidence

#### Screenshot 1 — Output of `git init` inside `CodeTrack` showing "Initialized empty Git repository"

Add your screenshot here.

---

#### Screenshot 2 — Output of `ls -a` showing the `.git` folder

Add your screenshot here.

---

### Notes

**1. What is the `.git` folder, and why does it matter?**

.git is a hidden folder at the root of a repository, and it is the repository. 
A plain folder is just files — whatever state they're in right now, and nothing else.
Adding .git turns that folder into a repository. Concretely, the folder gains:

History — every past version of every file, so you can look at or return to any point.
Branches — parallel lines of work in the same folder, switchable in a moment.
A staging area — you choose what goes into the next commit rather than saving everything at once.
Remotes — a recorded link to GitHub or similar, so push and pull know where to go.

Everything above lives inside .git. So the folder isn't "connected to git" in some external sense — it contains git. Move the folder anywhere, rename it, copy it to another machine: still a repo. Delete .git: instantly an ordinary folder again, with your current files intact but the past erased.

---

# Task 2 — Configure Git Identity Locally (Repository-Only)

## Goal

Set your Git username and email for the `CodeTrack` repository only, using `git config --local`.

### Evidence

#### Screenshot 3 — Output of `git config --local --list` showing your `user.name` and `user.email`

Add your screenshot here.

---

# Task 3 — Configure Git Identity Globally

## Goal

Set a global Git username and email for this machine using `git config --global`. Note that CodeTrack's local settings still take priority over these.

### Evidence

#### Screenshot 4 — Output of `git config --global --list` showing your `user.name` and `user.email`

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots
- Do not expose passwords, access tokens, or private keys

---

# Completion Checklist

- [✅] `CodeTrack` folder created and initialized as a Git repository (Screenshots 1–2)
- [✅] Explanation of the `.git` folder written in your own words
- [✅] Local `user.name` and `user.email` configured and verified (Screenshot 3)
- [✅] Global `user.name` and `user.email` configured and verified (Screenshot 4)
- [✅] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
