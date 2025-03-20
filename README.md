import streamlit as st
import openai
from fpdf import FPDF

# Set your OpenAI API key
OPENAI_API_KEY = "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"
openai.api_key = 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

# Function to generate resume using AI
def generate_resume(name, email, phone, skills, experience, education):
    prompt = f"""
    Create a professional resume for {name}.

    Contact Information:
    Email: {email}
    Phone: {phone}

    Skills:
    {skills}

    Work Experience:
    {experience}

    Education:
    {education}
    """

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )

    return response["choices"][0]["message"]["content"]

# Function to save resume as PDF
def save_as_pdf(resume_text, filename="resume.pdf"):
    pdf = FPDF()
    pdf.set_auto_page_break(auto=True, margin=15)
    pdf.add_page()
    pdf.set_font("Arial", size=12)
    
    for line in resume_text.split("\n"):
        pdf.cell(200, 10, txt=line, ln=True, align="L")
    
    pdf.output(filename)

# Streamlit UI
st.title("AI Resume Maker")
st.write("Generate a professional resume with AI.")

name = st.text_input("Full Name")
email = st.text_input("Email")
phone = st.text_input("Phone Number")
skills = st.text_area("Skills (comma-separated)")
experience = st.text_area("Work Experience")
education = st.text_area("Education")

if st.button("Generate Resume"):
    if name and email and phone and skills and experience and education:
        resume_text = generate_resume(name, email, phone, skills, experience, education)
        st.text_area("Generated Resume", resume_text, height=300)
        
        save_as_pdf(resume_text)
        with open("resume.pdf", "rb") as file:
            st.download_button("Download Resume PDF", file, "resume.pdf", "application/pdf")
    else:
        st.warning("Please fill in all fields.")

st.markdown("### Developed by [ChatGPT](https://www.linkedin.com/in/chatgpt/)")  # Replace with your LinkedIn
