 2311CS030565-545--Resume-nerd
RESUME NERD IS USED PROPOSE OF CV AND PROFESSIONAL PROPOSE

from collections import deque

class Resume:
    def _init_(self, name, skills, experience):
        self.name = name
        self.skills = skills
        self.experience = experience

    def _repr_(self):
        return f"{self.name} | Skills: {', '.join(self.skills)} | Experience: {self.experience} years"

class ResumeManager:
    def _init_(self):
        self.resumes = []  # List of resumes
        self.resume_versions = {}  # Stack to store resume versions
        self.job_applications = deque()  # Queue for job applications

    def add_resume(self, resume):
        self.resumes.append(resume)
        self.resume_versions[resume.name] = []  # Initialize stack for undo

    def update_resume(self, name, new_skills, new_experience):
        for resume in self.resumes:
            if resume.name == name:
                # Save previous version in stack
                self.resume_versions[name].append((resume.skills, resume.experience))
                resume.skills = new_skills
                resume.experience = new_experience
                print(f"Updated resume for {name}")
                return
        print("Resume not found.")

    def undo_update(self, name):
        if name in self.resume_versions and self.resume_versions[name]:
            prev_skills, prev_experience = self.resume_versions[name].pop()
            for resume in self.resumes:
                if resume.name == name:
                    resume.skills = prev_skills
                    resume.experience = prev_experience
                    print(f"Undo successful for {name}")
                    return
        print("No previous version found.")

    def apply_for_job(self, resume):
        self.job_applications.append(resume)

    def process_job_application(self):
        if self.job_applications:
            processed = self.job_applications.popleft()
            print(f"Processed job application for {processed.name}")
        else:
            print("No job applications to process.")

    def binary_search_resume(self, target_name):
        """Binary Search Algorithm (assumes sorted list)"""
        self.resumes.sort(key=lambda x: x.name)  # Ensure resumes are sorted
        low, high = 0, len(self.resumes) - 1

        while low <= high:
            mid = (low + high) // 2
            if self.resumes[mid].name == target_name:
                return self.resumes[mid]
            elif self.resumes[mid].name < target_name:
                low = mid + 1
            else:
                high = mid - 1
        return "Resume not found."

    def display_resumes(self):
        for resume in self.resumes:
            print(resume)

# Example Usage
manager = ResumeManager()
r1 = Resume("Alice", ["Python", "Machine Learning"], 5)
r2 = Resume("Bob", ["Java", "Spring Boot"], 3)
r3 = Resume("Charlie", ["JavaScript", "React"], 4)

manager.add_resume(r1)
manager.add_resume(r2)
manager.add_resume(r3)

manager.display_resumes()
print("\nUpdating Resume...")
manager.update_resume("Alice", ["Python", "AI"], 6)

print("\nUndo Update...")
manager.undo_update("Alice")

print("\nApplying for jobs...")
manager.apply_for_job(r1)
manager.apply_for_job(r2)

print("\nProcessing applications...")
manager.process_job_application()
manager.process_job_application()

print("\nSearching for a resume using Binary Search...")
print(manager.binary_search_resume("Charlie"))
