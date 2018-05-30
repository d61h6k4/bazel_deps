# -*- python -*-

# Copyright 2018 Josh Pieper, jjp@pobox.com.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("@com_github_mjbots_bazel_deps//tools/workspace:autoconf_config.bzl",
     "autoconf_config", "autoconf_standard_defines")
load("@com_github_mjbots_bazel_deps//tools/workspace:template_file.bzl",
     "template_file")

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "config",
    includes = ["."],
)

cc_library(
    name = "sub_glib",
    hdrs = glob([
        "glib/*.h",
        "glib/libcharset/*.h",
        "glib/deprecated/*.h",
        "glib/pcre/*.h",
    ]) + [
        "glib/glibconfig.h",
    ],
    deps = [":config"],
    copts = [
        "-I$(GENDIR)/external/glib/glibprivate",
        "-DGLIB_COMPILATION",
        "-DGLIB_CHARSETALIAS_DIR=\\\"XXX\\\"",
        "-Wno-int-conversion",
        "-Wno-tautological-constant-out-of-range-compare",
    ],
    includes = ["glib"],
    srcs = [
        "glibprivate/config.h",
    ] + ["glib/" + x for x in [
        "garray.c",
	"gasyncqueue.c",
	"gasyncqueueprivate.h",
	"gatomic.c",
	"gbacktrace.c",
	"gbase64.c",
	"gbitlock.c",
	"gbookmarkfile.c",
	"gbsearcharray.h",
	"gbytes.c",
	"gbytes.h",
	"gcharset.c",
	"gcharsetprivate.h",
	"gchecksum.c",
	"gconvert.c",
	"gdataset.c",
	"gdatasetprivate.h",
	"gdate.c",
	"gdatetime.c",
	"gdir.c",
	"genviron.c",
	"gerror.c",
	"gfileutils.c",
	"ggettext.c",
	"ghash.c",
	"ghmac.c",
	"ghook.c",
	"ghostutils.c",
	"giochannel.c",
        "giounix.c",
	"gkeyfile.c",
	"glibintl.h",
	"glib_trace.h",
	"glib-init.h",
	"glib-init.c",
	"glib-private.h",
	"glib-private.c",
        "glib-unix.c",
	"glist.c",
	"gmain-internal.h",
	"gmain.c",
	"gmappedfile.c",
	"gmarkup.c",
	"gmem.c",
	"gmessages.c",
	"gmirroringtable.h",
	"gnode.c",
	"goption.c",
	"gpattern.c",
	"gpoll.c",
	"gprimes.c",
	"gqsort.c",
	"gquark.c",
	"gqueue.c",
	"grand.c",
	"gregex.c",
	"gscanner.c",
	"gscripttable.h",
	"gsequence.c",
	"gshell.c",
	"gslice.c",
	"gslist.c",
        "gspawn.c",
	"gstdio.c",
	"gstdioprivate.h",
	"gstrfuncs.c",
	"gstring.c",
	"gstringchunk.c",
	"gtestutils.c",
	"gthread.c",
	"gthreadprivate.h",
	"gthreadpool.c",
        "gthread-posix.c",
	"gtimer.c",
	"gtimezone.c",
	"gtranslit.c",
	"gtranslit-data.h",
	"gtrashstack.c",
	"gtree.c",
	"guniprop.c",
	"gutf8.c",
	"gunibreak.h",
	"gunibreak.c",
	"gunichartables.h",
	"gunicollate.c",
	"gunicomp.h",
	"gunidecomp.h",
	"gunidecomp.c",
	"gunicodeprivate.h",
	"gurifuncs.c",
	"gutils.c",
	"guuid.c",
	"gvariant.h",
	"gvariant.c",
	"gvariant-core.h",
	"gvariant-core.c",
	"gvariant-internal.h",
	"gvariant-parser.c",
	"gvariant-serialiser.h",
	"gvariant-serialiser.c",
	"gvarianttypeinfo.h",
	"gvarianttypeinfo.c",
	"gvarianttype.c",
	"gversion.c",
	"gwakeup.h",
	"gwakeup.c",
	"gprintf.c",
	"gprintfint.h",
	"valgrind.h",
        "libcharset/localcharset.c",
    ]],
)

cc_library(
    name = "gobject",
    hdrs = glob([
        "gobject/*.h",
    ]),
    deps = [
        ":sub_glib",
        "@libffi",
    ],
    copts = [
        "-I$(GENDIR)/external/glib/glibprivate",
        "-DGOBJECT_COMPILATION",
        "-Iexternal/glib/glib",
        "-Wno-int-conversion",
    ],
    srcs = [
        "glibprivate/config.h",
    ] + ["gobject/" + x for x in [
        "gatomicarray.c",
        "gbinding.c",
        "gboxed.c",
        "gclosure.c",
        "genums.c",
        "gmarshal.c",
        "gobject.c",
        "gobject_trace.h",
        "gparam.c",
        "gparamspecs.c",
        "gsignal.c",
        "gsourceclosure.c",
        "gtype.c",
        "gtypemodule.c",
        "gtypeplugin.c",
        "gvalue.c",
        "gvaluearray.c",
        "gvaluetransform.c",
        "gvaluetypes.c",
    ]],
)

template_file(
    name = "gmodule/gmoduleconf.h",
    src = "gmodule/gmoduleconf.h.in",
    substitutions = {
        "@G_MODULE_IMPL@": "G_MODULE_IMPL_DL",
        "@G_MODULE_HAVE_DLERROR@": "1",
        "@G_MODULE_NEED_USCORE@": "0",
        "@G_MODULE_BROKEN_RTLD_GLOBAL@": "0",
    },
)

cc_library(
    name = "gmodule",
    hdrs = glob(["gmodule/*.h"]),
    srcs = [
        "gmodule/gmodule.c",
        "gmodule/gmoduleconf.h",
        "glibprivate/config.h",
    ],
    textual_hdrs = [
        "gmodule/gmodule-dl.c",
    ],
    copts = [
        "-I$(GENDIR)/external/glib/glibprivate",
    ],
    includes = ["gmodule"],
    deps = [":sub_glib"],
)

cc_library(
    name = "gio_xdgmime",
    copts = [
        "-I$(GENDIR)/external/glib/glibprivate",
        "-DGIO_COMPILATION",
        "-DXDG_PREFIX=_gio_xdg",
    ],
    hdrs = glob(["gio/xdgmime/*.h"]),
    srcs = [
        "glibprivate/config.h",
    ] + ["gio/xdgmime/" + x for x in [
        "xdgmime.c",
        "xdgmimealias.c",
        "xdgmimecache.c",
        "xdgmimeglob.c",
        "xdgmimeicon.c",
        "xdgmimeint.c",
        "xdgmimemagic.c",
        "xdgmimeparent.c",
    ]],
)

cc_library(
    name = "gio_headers",
    hdrs = glob(["gio/*.h"]),
    deps = [
        ":gobject",
        ":gmodule",
        ":gio_xdgmime",
    ],
    includes = ["gio"],
)

SETTINGS_BASE_SOURCES = ["gio/" + x for x in [
    "gvdb/gvdb-format.h",
    "gvdb/gvdb-reader.h",
    "gvdb/gvdb-reader.c",
    "gdelayedsettingsbackend.h",
    "gdelayedsettingsbackend.c",
    "gkeyfilesettingsbackend.c",
    "gmemorysettingsbackend.c",
    "gnullsettingsbackend.c",
    "gsettingsbackendinternal.h",
    "gsettingsbackend.c",
    "gsettingsschema.h",
    "gsettingsschema-internal.h",
    "gsettingsschema.c",
    "gsettings-mapping.h",
    "gsettings-mapping.c",
    "gsettings.c",
]]

SETTINGS_SOURCES = SETTINGS_BASE_SOURCES

APPLICATION_SOURCES = ["gio/" + x for x in [
    "gapplication.c",
    "gapplicationcommandline.c",
    "gapplicationimpl-dbus.c",
    "gapplicationimpl.h",
    "gactiongroup.c",
    "gactionmap.c",
    "gsimpleactiongroup.c",
    "gremoteactiongroup.c",
    "gactiongroupexporter.c",
    "gdbusactiongroup-private.h",
    "gdbusactiongroup.c",
    "gaction.c",
    "gpropertyaction.c",
    "gsimpleaction.c",
    "gmenumodel.c",
    "gmenu.c",
    "gmenuexporter.c",
    "gdbusmenumodel.c",
    "gnotification-private.h",
    "gnotificationbackend.h",
    "gnotification.c",
    "gnotificationbackend.c",
]]

# We're going to try and get away without this for now.
GDBUS_SOURCES = ["gio/" + x for x in [
  'gdbusauthobserver.h',
  'gcredentials.h',
  'gdbusutils.h',
  'gdbuserror.h',
  'gdbusaddress.h',
  'gdbusconnection.h',
  'gdbusmessage.h',
  'gdbusnameowning.h',
  'gdbusnamewatching.h',
  'gdbusproxy.h',
  'gdbusintrospection.h',
  'gdbusmethodinvocation.h',
  'gdbusserver.h',
  'gdbusinterface.h',
  'gdbusinterfaceskeleton.h',
  'gdbusobject.h',
  'gdbusobjectskeleton.h',
  'gdbusobjectproxy.h',
  'gdbusobjectmanager.h',
  'gdbusobjectmanagerclient.h',
  'gdbusobjectmanagerserver.h',
  'gtestdbus.h',

  'gdbusutils.c',
  'gdbusaddress.c',
  'gdbusauthobserver.c',
  'gdbusauth.c',
  'gdbusauthmechanism.c',
  'gdbusauthmechanismanon.c',
  'gdbusauthmechanismexternal.c',
  'gdbusauthmechanismsha1.c',
  'gdbuserror.c',
  'gdbusconnection.c',
  'gdbusmessage.c',
  'gdbusnameowning.c',
  'gdbusnamewatching.c',
  'gdbusproxy.c',
  'gdbusprivate.c',
  'gdbusintrospection.c',
  'gdbusmethodinvocation.c',
  'gdbusserver.c',
  'gdbusinterface.c',
  'gdbusinterfaceskeleton.c',
  'gdbusobject.c',
  'gdbusobjectskeleton.c',
  'gdbusobjectproxy.c',
  'gdbusobjectmanager.c',
  'gdbusobjectmanagerclient.c',
  'gdbusobjectmanagerserver.c',
  'gtestdbus.c',
]]

LOCAL_SOURCES = ["gio/" + x for x in [
    "ghttpproxy.c",
    "ghttpproxy.h",
    "glocalfile.c",
    "glocalfile.h",
    "glocalfileprivate.h",
    "glocalfileenumerator.c",
    "glocalfileenumerator.h",
    "glocalfileinfo.c",
    "glocalfileinfo.h",
    "glocalfileinputstream.c",
    "glocalfileinputstream.h",
    "glocalfilemonitor.c",
    "glocalfilemonitor.h",
    "glocalfileoutputstream.c",
    "glocalfileoutputstream.h",
    "glocalfileiostream.c",
    "glocalfileiostream.h",
    "glocalvfs.c",
    "glocalvfs.h",
    "gsocks4proxy.c",
    "gsocks4proxy.h",
    "gsocks4aproxy.c",
    "gsocks4aproxy.h",
    "gsocks5proxy.c",
    "gsocks5proxy.h",
    "thumbnail-verify.h",
    "thumbnail-verify.c",
]]

template_file(
    name = "gio/gnetworking.h",
    src = "gio/gnetworking.h.in",
    substitutions = {
        "@WSPIAPI_INCLUDE@": "",
        "@NAMESER_COMPAT_INCLUDE@": "",
    },
)

UNIX_SOURCES = ["gio/" + x for x in [
    "gfiledescriptorbased.c",
    "gunixconnection.c",
    "gunixcredentialsmessage.c",
    "gunixfdlist.c",
    "gunixfdmessage.c",
    "gunixmount.c",
    "gunixmount.h",
    "gunixmounts.c",
    "gunixsocketaddress.c",
    "gunixvolume.c",
    "gunixvolume.h",
    "gunixvolumemonitor.c",
    "gunixvolumemonitor.h",
    "gunixinputstream.c",
    "gunixoutputstream.c",
    "gcontenttypeprivate.h",
    "gfdonotificationbackend.c",
    "ggtknotificationbackend.c",
    "gportalnotificationbackend.c",
    "gdocumentportal.c",
    "gdocumentportal.h",
    "gopenuriportal.c",
    "gopenuriportal.h",
    "gportalsupport.c",
    "gportalsupport.h",
    "gnetworkmonitorportal.c",
    "gnetworkmonitorportal.h",
    "gproxyresolverportal.c",
    "gproxyresolverportal.h",
    "xdp-dbus.c",
    "xdp-dbus.h",
]]

CONTENTTYPE_SOURCES = ["gio/" + x for x in [
    "gcontenttype.c",
]]

APPINFO_SOURCES = ["gio/" + x for x in [
    "gdesktopappinfo.c",
]]

GIO_BASE_SOURCES = ["gio/" + x for x in [
    "gappinfo.c",
    "gappinfoprivate.h",
    "gasynchelper.c",
    "gasynchelper.h",
    "gasyncinitable.c",
    "gasyncresult.c",
    "gbufferedinputstream.c",
    "gbufferedoutputstream.c",
    "gbytesicon.c",
    "gcancellable.c",
    "gcharsetconverter.c",
    "gcontextspecificgroup.c",
    "gcontextspecificgroup.h",
    "gconverter.c",
    "gconverterinputstream.c",
    "gconverteroutputstream.c",
    "gcredentials.c",
    "gcredentialsprivate.h",
    "gdatagrambased.c",
    "gdatainputstream.c",
    "gdataoutputstream.c",
    "gdrive.c",
    "gdummyfile.h",
    "gdummyfile.c",
    "gdummyproxyresolver.c",
    "gdummyproxyresolver.h",
    "gdummytlsbackend.c",
    "gdummytlsbackend.h",
    "gemblem.h",
    "gemblem.c",
    "gemblemedicon.h",
    "gemblemedicon.c",
    "gfile.c",
    "gfileattribute.c",
    "gfileattribute-priv.h",
    "gfileenumerator.c",
    "gfileicon.c",
    "gfileinfo.c",
    "gfileinfo-priv.h",
    "gfileinputstream.c",
    "gfilemonitor.c",
    "gfilenamecompleter.c",
    "gfileoutputstream.c",
    "gfileiostream.c",
    "gfilterinputstream.c",
    "gfilteroutputstream.c",
    "gicon.c",
    "ginetaddress.c",
    "ginetaddressmask.c",
    "ginetsocketaddress.c",
    "ginitable.c",
    "ginputstream.c",
    "gio_trace.h",
    "gioenums.h",
    "gioerror.c",
    "giomodule.c",
    "giomodule-priv.c",
    "giomodule-priv.h",
    "gioscheduler.c",
    "giostream.c",
    "gioprivate.h",
    "giowin32-priv.h",
    "gloadableicon.c",
    "gmount.c",
    "gmemoryinputstream.c",
    "gmemoryoutputstream.c",
    "gmountoperation.c",
    "gnativevolumemonitor.c",
    "gnativevolumemonitor.h",
    "gnativesocketaddress.c",
    "gnativesocketaddress.h",
    "gnetworkaddress.c",
    "gnetworking.c",
    "gnetworking.h",
    "gnetworkingprivate.h",
    "gnetworkmonitor.c",
    "gnetworkmonitorbase.c",
    "gnetworkmonitorbase.h",
    "gnetworkservice.c",
    "goutputstream.c",
    "gpermission.c",
    "gpollableinputstream.c",
    "gpollableoutputstream.c",
    "gpollableutils.c",
    "gpollfilemonitor.c",
    "gpollfilemonitor.h",
    "gproxy.c",
    "gproxyaddress.c",
    "gproxyaddressenumerator.c",
    "gproxyresolver.c",
    "gresolver.c",
    "gresource.c",
    "gresourcefile.c",
    "gresourcefile.h",
    "gseekable.c",
    "gsimpleasyncresult.c",
    "gsimpleiostream.c",
    "gsimplepermission.c",
    "gsocket.c",
    "gsocketaddress.c",
    "gsocketaddressenumerator.c",
    "gsocketclient.c",
    "gsocketconnectable.c",
    "gsocketconnection.c",
    "gsocketcontrolmessage.c",
    "gsocketinputstream.c",
    "gsocketinputstream.h",
    "gsocketlistener.c",
    "gsocketoutputstream.c",
    "gsocketoutputstream.h",
    "gsubprocesslauncher.c",
    "gsubprocess.c",
    "gsubprocesslauncher-private.h",
    "gsocketservice.c",
    "gsrvtarget.c",
    "gsimpleproxyresolver.c",
    "gtask.c",
    "gtcpconnection.c",
    "gtcpwrapperconnection.c",
    "gthreadedsocketservice.c",
    "gthemedicon.c",
    "gthreadedresolver.c",
    "gthreadedresolver.h",
    "gtlsbackend.c",
    "gtlscertificate.c",
    "gtlsclientconnection.c",
    "gtlsconnection.c",
    "gtlsdatabase.c",
    "gtlsfiledatabase.c",
    "gtlsinteraction.c",
    "gtlspassword.c",
    "gtlsserverconnection.c",
    "gdtlsconnection.c",
    "gdtlsclientconnection.c",
    "gdtlsserverconnection.c",
    "gunionvolumemonitor.c",
    "gunionvolumemonitor.h",
    "gvfs.c",
    "gvolume.c",
    "gvolumemonitor.c",
    "gzlibcompressor.c",
    "gzlibdecompressor.c",
    "gmountprivate.h",
    "gioenumtypes.h",
    "gioenumtypes.c",
    "glistmodel.c",
    "gliststore.c",

    "gnetworkmonitornetlink.c",
    "gnetworkmonitornm.c",
]] + APPLICATION_SOURCES + GDBUS_SOURCES + LOCAL_SOURCES

LIBGIO_SOURCES = GIO_BASE_SOURCES + APPINFO_SOURCES + CONTENTTYPE_SOURCES + UNIX_SOURCES + SETTINGS_SOURCES

cc_library(
    name = "gio",
    copts = [
        "-I$(GENDIR)/external/glib/glibprivate",
        "-DGIO_COMPILATION",
        "-DGIO_MODULE_DIR=\\\"XXX/gio/modules\\\"",
        "-Wno-implicit-function-declaration",
        "-Wno-int-conversion",
        "-Wno-tautological-constant-out-of-range-compare",
        "-Wno-self-assign",
    ],
    deps = [
        ":gio_headers",
        "@pcre",
    ],
    srcs = [
        "glibprivate/config.h",
    ] + LIBGIO_SOURCES,
    textual_hdrs = [
        "gio/strinfo.c",
    ],
    linkopts = [
        "-lresolv",
    ],
)

BASE_DEFINES =[
    "ALIGNOF_GUINT32=4",
    "ALIGNOF_GUINT64=8",
    "ENABLE_NLS",
    "GETTEXT_PACKAGE=\"glib20\"",
    "GLIB_BINARY_AGE=5701",
    "GLIB_INTERFACE_AGE=0",
    "GLIB_LOCALE_DIR=\"XXX\"",
    "GLIB_MAJOR_VERSION=2",
    "GLIB_MINOR_VERSION=57",
    "GLIB_MICRO_VERSION=1",
    "POSIX_MEMALIGN_WITH_COMPLIANT_ALLOCS",
    "STRERROR_R_CHAR_P",
    "THREADS_POSIX",
    "USE_STATFS",
    "__EXTENSIONS__",
    "_GLIB_EXTERN=__attribute__((visibility(\"default\"))) extern",
]

autoconf_config(
    name = "glibprivate/config.h",
    src = "config.h.in",
    package = "glib",
    version = "2.57.1",
    defines = autoconf_standard_defines + BASE_DEFINES,
)

BASE_SUBSTITUTIONS = {
    "#mesondefine GLIB_HAVE_ALLOCA_H": "#define GLIB_HAVE_ALLOCA_H",
    "#mesondefine GLIB_USING_SYSTEM_PRINTF": "#define GLIB_USING_SYSTEM_PRINTF",
    "#mesondefine GLIB_STATIC_COMPILATION": "",
    "#mesondefine GOBJECT_STATIC_COMPILATION": "",
    "@gint16@": "short",
    "@gint16_modifier@": "\"h\"",
    "@gint16_format@": "\"hi\"",
    "@guint16_format@": "\"hu\"",
    "@gint32@": "int",
    "@gint32_modifier@": "\"\"",
    "@gint32_format@": "\"i\"",
    "@guint32_format@": "\"u\"",
    "@glib_extension@": "",
    "@gint64@": "long long",
    "@gint64_constant@": "(val##LL)",
    "@guint64_constant@": "(val##ULL)",
    "@gint64_modifier@": "\"ll\"",
    "@gint64_format@": "\"lli\"",
    "@guint64_format@": "\"llu\"",

    "@glib_msize_type@": "LONG",
    "@g_pollfd_format@": "\"%d\"",
    "@glib_gpi_cast@": "(glong)",
    "@glib_gpui_cast@": "(gulong)",

    "@glib_intptr_type_define@": "long",
    "@gintptr_modifier@": "\"l\"",
    "@gintptr_format@": "\"li\"",
    "@guintptr_format@": "\"lu\"",

    "@GLIB_MAJOR_VERSION@": "2",
    "@GLIB_MINOR_VERSION@": "57",
    "@GLIB_MICRO_VERSION@": "1",

    "@glib_os@": "#define G_OS_UNIX",
    "@glib_vacopy@": """
#define G_VA_COPY va_copy
#define G_VA_COPY_AS_ARRAY 1
    """,
    "@g_have_iso_c_varargs@": """
#ifndef __cplusplus
# define G_HAVE_ISO_VARARGS 1
#endif
    """,
    "@g_have_iso_cxx_varargs@": """
#ifdef __cplusplus
# define G_HAVE_ISO_VARARGS 1
#endif
    """,

    "#mesondefine G_HAVE_GROWING_STACK": "#define G_HAVE_GNUC_VARARGS 1",
    "#mesondefine G_HAVE_GNUC_VISIBILITY": "#define G_HAVE_GROWING_STACK 0",

    "@g_threads_impl_def@": "POSIX",

    "#mesondefine G_ATOMIC_OP_MEMORY_BARRIER_NEEDED": "",
    "#mesondefine G_ATOMIC_LOCK_FREE": "#define G_ATOMIC_LOCK_FREE",


    "@g_bs_native@": "LE",
    "@g_bs_alien@": "BE",

    "@glongbits@": "64",
    "@gintbits@": "32",
    "@gsizebits@": "64",
    "@g_byte_order@": "G_LITTLE_ENDIAN",

    "@g_pollin@": "1",
    "@g_pollout@": "4",
    "@g_pollpri@": "2",
    "@g_pollhup@": "16",
    "@g_pollerr@": "8",
    "@g_pollnval@": "32",

    "@g_module_suffix@": "so",

    "@g_pid_type@": "int",
    "@g_pid_format@": "\"i\"",

    "@g_af_unix@": "1",
    "@g_af_inet@": "2",
    "@g_af_inet6@": "10",
    "@g_msg_oob@": "1",
    "@g_msg_peek@": "2",
    "@g_msg_dontroute@": "4",

    "@g_dir_separator@": "/",
    "@g_searchpath_separator@": ":",
}

template_file(
    name = "glib/glibconfig.h",
    src = "glib/glibconfig.h.in",
    substitution_list = select({
        "@com_github_mjbots_bazel_deps//conditions:long_8bytes" : [
            "@glib_void_p@=8",
            "@glib_long@=8",
        ],
        "@com_github_mjbots_bazel_deps//conditions:long_4bytes" : [
            "@glib_void_p@=4",
            "@glib_long@=4",
        ],
    }) + select({
        "@com_github_mjbots_bazel_deps//conditions:sizet_8bytes" : [
            "@glib_size_t@=8",
            "@glib_ssize_t@=8",
        ],
        "@com_github_mjbots_bazel_deps//conditions:sizet_4bytes" : [
            "@glib_size_t@=4",
            "@glib_ssize_t@=4",
        ],
    }) + select({
        "@com_github_mjbots_bazel_deps//conditions:sizet_long" : [
            "@glib_size_type_define@=long",

            "@gsize_modifier@=\"l\"",
            "@gssize_modifier@=\"l\"",
            "@gsize_format@=\"lu\"",
            "@gssize_format@=\"li\"",
        ],
        "@com_github_mjbots_bazel_deps//conditions:sizet_int" : [
            "@glib_size_type_define@=int",

            "@gsize_modifier@=\"\"",
            "@gssize_modifier@=\"\"",
            "@gsize_format@=\"u\"",
            "@gssize_format@=\"i\"",
        ],
    }) + ['%s=%s' % (k, v) for k, v in BASE_SUBSTITUTIONS.items()],
)

cc_library(
    name = "glib",
    deps = [
        ":sub_glib",
        ":gobject",
        ":gmodule",
        ":gio",
    ],
)

template_file(
    name = "gobject/glib-mkenums.py",
    src = "gobject/glib-mkenums.in",
    substitutions = {
        "@PYTHON@" : "python",
        "@VERSION@" : "XXX",
    },
)

py_binary(
    name = "glib-mkenums",
    srcs = ["gobject/glib-mkenums.py"],
)
