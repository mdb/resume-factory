# Resume Factory

Create HTML and PDF versions of your resume & upload the files to S3. Use Rake, HAML, SCSS, and JavaScript.

Problem: Maintaining multiple formats and multiple versions of a resume is painful. You're a developer; why haggle with Microsoft Word or Adobe InDesign?

Solution: Leverage git, [middleman](http://middlemanapp.com), and Amazon S3.

## Getting started

Get the source code, set up your environment, and run the development server locally.

1. Fork resume-factory and:

    git clone https://github.com/your_username/resume-factory.git

    cd resume-factory
  
    bundle
  
    rake server

2. Edit the HAML, SCSS, and JS in the source directory to customize your resume.

3. Commit your changes to version control.

4. Want to maintain seperate versions of your resume, depending on the type of position you're applying for? Branch, tag, and release 'em.

5. Comple to static HTML & a PDF:

    rake

  The default rake tasks builds a 'resume' directory within which is a PDF and HTML version of your resume.

6. Publish your resume to Amazon S3

    rake upload

  Note that 'rake upload' requires the following environment variables be set in accordance with your S3 credentials:

- AWS_ACCESS_KEY - your AWS access key
- AWS_SECRET_KEY - your AWS secret key
- FOG_DIRECTORY - your AWS bucket

## About the stylesheets

base.scss controls styles shared by both the HTML and PDF versions of your resume.

screen.scss controls HTML-specific styles.

pdf.scss controls PDF-specific styles.

## Customizing the PDF filename

Edit line 7 of the Rakefile to customize your PDF filename.

## TODO

- support the ability to house configuration & AWS credentials in .resumerc or resume_config.yml files
