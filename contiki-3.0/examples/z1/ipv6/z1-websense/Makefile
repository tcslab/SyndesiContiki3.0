all: z1-websense

CONTIKI=../../../..

SMALL=1

APPS += webserver webbrowser burn-nodeid z1-websense z1-websense+phidget

CFLAGS += -DPROJECT_CONF_H=\"project-conf.h\"
CONTIKI_SOURCEFILES += wget.c
PROJECTDIRS += ../../../ipv6/rpl-border-router
PROJECT_SOURCEFILES += httpd-simple.c

CONTIKI_WITH_IPV6 = 1
include $(CONTIKI)/Makefile.include

$(CONTIKI)/tools/tunslip6:	$(CONTIKI)/tools/tunslip6.c
	(cd $(CONTIKI)/tools && $(MAKE) tunslip6)

connect-router:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 aaaa::1/64

connect-router-cooja:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 -a 127.0.0.1 aaaa::1/64

CUSTOM_RULE_LINK=1 
%.$(TARGET): %.co $(PROJECT_OBJECTFILES) $(PROJECT_LIBRARIES) contiki-$(TARGET).a $(LD) $(LDFLAGS) $(TARGET_STARTFILES) ${filter-out %.a,$^} ${filter %.a,$^} $(TARGET_LIBFILES) -o $@ -lm
