# Resume Factory

Problem: maintaining multiple formats and multiple versions of a resume is painful.

Solution: Leverage git, [middleman](http://middlemanapp.com), and Amazon S3 to to built and publish individually-styled HTML and PDF versions of your resume from a HAML template, using SCSS & JavaScript.

## Getting started

Get the source code, set up your environment, and run the server locally:

Fork resume-factory and:

    git clone https://github.com/your_username/resume-factory.git
    cd resume-factory
    bundle
    rake server

Then edit the HAML, SCSS, and JS in the source directory to customize your resume.

Commit your changes to version control.

What to maintain seperate versions of your resume, depending on the type of position you're applying for? Branch, tag, and release multiple versions.

## About the stylesheets

base.scss controls styles shared by both the HTML and PDF versions of your resume.

screen.scss controls HTML-specific styles.

pdf.scss controls PDF-specific styles.

## Compile to static HTML and a PDF

The default rake tasks builds a 'resume' directory within which is a PDF and HTML version of your resume.

    rake

## Customizing the PDF filename

Edit line 7 of the Rakefile to customize your PDF filename.

## Publish your resume to Amazon S3

    rake upload

Note that 'rake upload' requires the following environment variables be set in accordance with your S3 credentials:

- AWS_ACCESS_KEY - your AWS access key
- AWS_SECRET_KEY - your AWS secret key
- FOG_DIRECTORY - your AWS bucket

## TODO

- support the ability to house configuration & AWS credentials in .resumerc or resume_config.yml files
