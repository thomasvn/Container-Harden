FROM httpd:2.4

# Test webpage
COPY ./public-html/ /usr/local/apache2/htdocs/

# STIG'd configuration files
COPY ./confs/* /usr/local/apache2/conf/