all: ErO2-App
# Use this target explicitly if requried: er-plugtest-server

CONTIKI=../..
CFLAGS += -DPROJECT_CONF_H=\"project-conf.h\"
PROJECT_SOURCEFILES += ero2manager.c
PROJECT_SOURCEFILES += ero2service.c
PROJECT_SOURCEFILES += ero2client.c
PROJECT_SOURCEFILES += ero2registry.c


# variable for root Makefile.include
WITH_UIP6=1
# for some platforms
UIP_CONF_IPV6=1


CFLAGS += -DRF_CHANNEL=25 #CHANGED BY STEF
# variable for this Makefile
# configure CoAP implementation (3|7) (er-coap-07 also supports CoAP draft 08)
WITH_COAP=18

# new variable since slip-radio
ifneq ($(TARGET), minimal-net)
UIP_CONF_RPL=1
else
# minimal-net does not support RPL under Linux and is mostly used to test CoAP only
${info INFO: compiling without RPL}
UIP_CONF_RPL=0
CFLAGS += -DUIP_CONF_ND6_DEF_MAXDADNS=0
CFLAGS += -DHARD_CODED_ADDRESS=\"fdfd::10\"
endif

# linker optimizations
SMALL=1

# REST framework, requires WITH_COAP
ifeq ($(WITH_COAP), 12)
${info INFO: compiling with CoAP-12}
CFLAGS += -DWITH_COAP=12
CFLAGS += -DREST=coap_rest_implementation
CFLAGS += -DUIP_CONF_TCP=0
APPS += er-coap-12
else ifeq ($(WITH_COAP), 18)
${info INFO: compiling with CoAP-18}
CFLAGS += -DWITH_COAP=18
CFLAGS += -DREST=coap_rest_implementation
CFLAGS += -DUIP_CONF_TCP=0
CFLAGS += -DUIP_CONF_BUFFER_SIZE=2560
APPS += er-coap
else ifeq ($(WITH_COAP), 3)
${info INFO: compiling with CoAP-03}
CFLAGS += -DWITH_COAP=3
CFLAGS += -DREST=coap_rest_implementation
CFLAGS += -DUIP_CONF_TCP=0
APPS += er-coap-03
else
${info INFO: compiling with HTTP}
CFLAGS += -DWITH_HTTP
CFLAGS += -DREST=http_rest_implementation
CFLAGS += -DUIP_CONF_TCP=1
APPS += er-http-engine
endif

APPS += erbium
APPS += rest-engine

# optional rules to get assembly
#CUSTOM_RULE_C_TO_OBJECTDIR_O = 1
#CUSTOM_RULE_S_TO_OBJECTDIR_O = 1

include $(CONTIKI)/Makefile.include

# optional rules to get assembly
#$(OBJECTDIR)/%.o: asmdir/%.S
#	$(CC) $(CFLAGS) -MMD -c $< -o $@
#	@$(FINALIZE_DEPENDENCY)
#
#asmdir/%.S: %.c
#	$(CC) $(CFLAGS) -MMD -S $< -o $@

# border router rules
$(CONTIKI)/tools/tunslip6:	$(CONTIKI)/tools/tunslip6.c
	(cd $(CONTIKI)/tools && $(MAKE) tunslip6)

connect-router:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 aaaa::1/64

connect-router-cooja:	$(CONTIKI)/tools/tunslip6
	sudo $(CONTIKI)/tools/tunslip6 -a 127.0.0.1 aaaa::1/64

tap0up:
	sudo ip address add fdfd::1/64 dev tap0
