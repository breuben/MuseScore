#=============================================================================
#  MuseScore
#  Linux Music Score Editor
#
#  Copyright (C) 2002-2013 by Werner Schweer and others
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License version 2.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, write to the Free Software
#  Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.
#=============================================================================

include (${PROJECT_SOURCE_DIR}/build/gch.cmake)

SET_SOURCE_FILES_PROPERTIES(revision.h PROPERTIES GENERATED TRUE)

include_directories(
      ${CMAKE_CURRENT_BINARY_DIR}
      ${PROJECT_SOURCE_DIR}
      ${PROJECT_SOURCE_DIR}/thirdparty
      ${QTSINGLEAPPLICATION_INCLUDE_DIRS}
      )

if (SCRIPT_INTERFACE)
      set (SCRIPT_FILES mscorePlugins pluginCreator.cpp qmledit.cpp pluginManager.cpp)
      set (SCRIPT_UI   pluginCreator.ui pluginManager.ui)
endif (SCRIPT_INTERFACE)

QT5_WRAP_UI (ui_headers
      insertmeasuresdialog.ui editinstrument.ui editstyle.ui instrdialog.ui instrwidget.ui
      measuresdialog.ui pagesettings.ui mixer.ui playpanel.ui prefsdialog.ui measureproperties.ui
      textpalette.ui  timedialog.ui symboldialog.ui  shortcutcapturedialog.ui  editdrumset.ui
      editstaff.ui voltaproperties.ui timesigproperties.ui
      instrwizard.ui timesigwizard.ui newwizard.ui aboutbox.ui aboutmusicxmlbox.ui
      transposedialog.ui lineproperties.ui excerptsdialog.ui  stafftext.ui tupletdialog.ui
      glissandoprop.ui articulation.ui metaedit.ui palette.ui select.ui
      synthcontrol.ui splitstaff.ui cellproperties.ui keyedit.ui selectinstr.ui
      fretdprops.ui editstafftype.ui sectionbreak.ui  bend.ui tremolobar.ui textproperties.ui
      editpitch.ui editstringdata.ui editraster.ui mediadialog.ui textstyle.ui albummanager.ui layer.ui
      omrpanel.ui startdialog.ui masterpalette.ui harmonyedit.ui pathlistdialog.ui
      note_groups.ui resourceManager.ui stafftypetemplates.ui
      startcenter.ui scorePreview.ui scoreBrowser.ui
      logindialog.ui uploadscoredialog.ui breaksdialog.ui

      importmidi/importmidi_panel.ui

      debugger/barline.ui
      debugger/harmony.ui
      debugger/box.ui
      debugger/keysig.ui
      debugger/rest.ui
      debugger/articulationbase.ui
      debugger/accidental.ui
      debugger/clef.ui
      debugger/slursegment.ui
      debugger/spanner.ui
      debugger/tremolo.ui
      debugger/beam.ui
      debugger/lyrics.ui
      debugger/linesegment.ui
      debugger/textline.ui
      debugger/line.ui
      debugger/slurtie.ui
      debugger/tie.ui
      debugger/tuplet.ui
      debugger/dynamic.ui
      debugger/hairpin.ui
      debugger/chord.ui
      debugger/chordrest.ui
      debugger/segment.ui
      debugger/measure.ui
      debugger/element.ui
      debugger/debugger.ui
      debugger/note.ui
      debugger/page.ui
      debugger/text.ui
      debugger/stem.ui
      debugger/system.ui
      debugger/timesig.ui

      inspector/inspector_element.ui
      inspector/inspector_vbox.ui
      inspector/inspector_articulation.ui
      inspector/inspector_spacer.ui
      inspector/inspector_hbox.ui
      inspector/inspector_segment.ui
      inspector/inspector_beam.ui
      inspector/inspector_note.ui
      inspector/inspector_rest.ui
      inspector/inspector_chord.ui
      inspector/inspector_group_element.ui
      inspector/inspector_image.ui
      inspector/inspector_lasso.ui
      inspector/inspector_volta.ui
      inspector/inspector_ottava.ui
      inspector/inspector_trill.ui
      inspector/inspector_hairpin.ui
      inspector/inspector_jump.ui
      inspector/inspector_marker.ui
      inspector/inspector_glissando.ui
      inspector/inspector_clef.ui
      inspector/inspector_timesig.ui
      inspector/inspector_keysig.ui
      inspector/inspector_barline.ui
      inspector/inspector_tuplet.ui
      inspector/inspector_accidental.ui
      inspector/inspector_tempotext.ui
      inspector/inspector_dynamic.ui
      inspector/inspector_line.ui
      inspector/inspector_textline.ui
      inspector/inspector_slur.ui
      inspector/inspector_text.ui
      inspector/inspector_ambitus.ui
      inspector/inspector_empty.ui
      inspector/inspector_fret.ui
      inspector/inspector_break.ui
      ${SCRIPT_UI}
      )

if (APPLE)
      QT5_ADD_RESOURCES (qrc_files musescore.qrc)
else (APPLE)
      QT5_ADD_RESOURCES (qrc_files musescore.qrc
            musescorefonts-MScore.qrc
            musescorefonts-Gootville.qrc
            musescorefonts-Bravura.qrc
            musescorefonts-MuseJazz.qrc
            musescorefonts-FreeSerif.qrc
            musescorefonts-Free.qrc)
endif (APPLE)


set (AUDIO "")
if (MINGW)
      set (resource_file ${PROJECT_BINARY_DIR}/resfile.o)
else (MINGW)
      if (USE_ALSA)
            set (AUDIO ${AUDIO} alsa.cpp mididriver.cpp)
      endif (USE_ALSA)
endif (MINGW)

if (USE_PORTAUDIO)
      set (AUDIO ${AUDIO} pa.cpp)
endif (USE_PORTAUDIO)

if (USE_PULSEAUDIO)
      set (AUDIO ${AUDIO} pulseaudio.cpp)
endif (USE_PULSEAUDIO)

if (USE_PORTMIDI)
      set (AUDIO ${AUDIO} pm.cpp)
endif (USE_PORTMIDI)

if (USE_JACK)
      set (AUDIO ${AUDIO} jackaudio.cpp jackweakapi.cpp)
endif (USE_JACK)

if (${CMAKE_SYSTEM} MATCHES "GNU-0.3")
      # include some cmake support file
      INCLUDE (Platform/GNU)
      set ( USE_ALSA 0 )
      set ( USE_PORTMIDI 0 )
endif (${CMAKE_SYSTEM} MATCHES "GNU-0.3")

if (${CMAKE_SYSTEM} MATCHES "kFreeBSD")
      # include some cmake support file
      INCLUDE (Platform/kFreeBSD)
      set ( USE_ALSA 0 )
      set ( USE_PORTMIDI 0 )
endif (${CMAKE_SYSTEM} MATCHES "kFreeBSD")

if (APPLE)
      set (ExecutableName mscore MACOSX_BUNDLE)
      set (MACOSX_BUNDLE_ICON_FILE      mscore.icns)
      set (MACOSX_BUNDLE_GUI_IDENTIFIER org.musescore.MuseScore)
      set (MACOSX_BUNDLE_LONG_VERSION_STRING ${MUSESCORE_VERSION_FULL})
      set (MACOSX_BUNDLE_BUNDLE_NAME MuseScore)
      set (MACOSX_BUNDLE_SHORT_VERSION_STRING ${MUSESCORE_VERSION})
      set (MACOSX_BUNDLE_BUNDLE_VERSION ${MUSESCORE_VERSION_FULL})
      set (MACOSX_BUNDLE_COPYRIGHT musescore.org)
else (APPLE)
      set (ExecutableName mscore)
endif (APPLE)

if (OMR)
      set(OMR_FILES omrpanel.cpp)
endif (OMR)

if (APPLE)
      file(GLOB_RECURSE INCS "*.h")
else (APPLE)
      set(INCS "")
endif (APPLE)

add_executable ( ${ExecutableName}
      ${qrc_files}
      ${ui_headers}
      ${PROJECT_BINARY_DIR}/all.h
      ${PCH}
      ${resource_file}
      ${INCS}

      recordbutton.h greendotbutton prefsdialog.h
      scoreview.cpp editinstrument.cpp editstyle.cpp
      icons.cpp importbww.cpp
      importmxml.cpp importmxmlpass1.cpp importmxmlpass2.cpp
      instrdialog.cpp instrwidget.cpp
      debugger/debugger.cpp menus.cpp
      musescore.cpp navigator.cpp pagesettings.cpp palette.cpp
      mixer.cpp playpanel.cpp selectionwindow.cpp preferences.cpp measureproperties.cpp
      seq.cpp textpalette.cpp
      timedialog.cpp symboldialog.cpp shortcutcapturedialog.cpp
      simplebutton.cpp musedata.cpp
      editdrumset.cpp editstaff.cpp voltaproperties.cpp
      timesigproperties.cpp newwizard.cpp transposedialog.cpp
      excerptsdialog.cpp metaedit.cpp magbox.cpp
      capella.cpp capxml.cpp exportaudio.cpp palettebox.cpp
      textproperties.cpp synthcontrol.cpp drumroll.cpp pianoroll.cpp piano.cpp
      pianoview.cpp drumview.cpp scoretab.cpp keyedit.cpp harmonyedit.cpp
      updatechecker.cpp importove.cpp ove.cpp ruler.cpp
      importgtp.cpp importgtp-gp4.cpp importgtp-gp5.cpp importgtp-gp6.cpp fotomode.cpp drumtools.cpp
      selinstrument.cpp texteditor.cpp editstafftype.cpp texttools.cpp
      editpitch.cpp editstringdata.cpp editraster.cpp pianotools.cpp mediadialog.cpp
      workspace.cpp exportmp3.cpp chordview.cpp
      album.cpp albummanager.cpp
      textstyle.cpp
      lineproperties.cpp stafftextproperties.cpp splitstaff.cpp
      tupletdialog.cpp glissandoproperties.cpp
      articulationprop.cpp textprop.cpp
      fretproperties.cpp sectionbreakprop.cpp
      bendproperties.cpp tremolobarprop.cpp file.cpp keyb.cpp osc.cpp
      layer.cpp selectdialog.cpp propertymenu.cpp shortcut.cpp bb.cpp
      inspector/inspector.cpp dragelement.cpp svggenerator.cpp
      inspector/inspectorBase.cpp inspector/inspectorBeam.cpp masterpalette.cpp
      inspector/inspectorGroupElement.cpp dragdrop.cpp inspector/inspectorImage.cpp
      inspector/inspectorFret.cpp
      inspector/inspectorText.cpp
      waveview.cpp helpBrowser.cpp inspector/inspectorLasso.cpp
      editelement.cpp inspector/inspectorVolta.cpp inspector/inspectorOttava.cpp enableplayforwidget.cpp
      inspector/inspectorTrill.cpp
      inspector/inspectorHairpin.cpp qmlplugin.cpp editlyrics.cpp
      musicxmlfonthandler.cpp musicxmlsupport.cpp exportxml.cpp importxml.cpp importxmlfirstpass.cpp
      savePositions.cpp inspector/inspectorJump.cpp inspector/inspectorMarker.cpp
      inspector/inspectorGlissando.cpp inspector/inspectorNote.cpp inspector/inspectorAmbitus.cpp
      paletteBoxButton.cpp driver.cpp exportmidi.cpp noteGroups.cpp
      pathlistdialog.cpp exampleview.cpp inspector/inspectorTextLine.cpp miconengine.cpp
      importmidi/importmidi.cpp
      importmidi/importmidi_panel.cpp importmidi/importmidi_operations.cpp
      importmidi/importmidi_model.cpp importmidi/importmidi_delegate.cpp
      importmidi/importmidi_meter.cpp importmidi/importmidi_quant.cpp
      importmidi/importmidi_tuplet.cpp importmidi/importmidi_chord.cpp
      importmidi/importmidi_swing.cpp importmidi/importmidi_fraction.cpp
      importmidi/importmidi_drum.cpp importmidi/importmidi_clef.cpp
      importmidi/importmidi_lrhand.cpp importmidi/importmidi_lyrics.cpp
      importmidi/importmidi_inner.cpp importmidi/importmidi_tie.cpp
      importmidi/importmidi_tuplet_voice.cpp importmidi/importmidi_beat.cpp
      importmidi/importmidi_tuplet_detect.cpp importmidi/importmidi_tuplet_filter.cpp
      importmidi/importmidi_tuplet_tonotes.cpp importmidi/importmidi_simplify.cpp
      importmidi/importmidi_voice.cpp importmidi/importmidi_view.cpp importmidi/importmidi_key.cpp
      importmidi/importmidi_tempo.cpp importmidi/importmidi_instrument.cpp
      importmidi/importmidi_chordname.cpp
      resourceManager.cpp downloadUtils.cpp
      textcursor.cpp continuouspanel.cpp accessibletoolbutton.cpp scoreaccessibility.cpp
      startcenter.cpp scoreBrowser.cpp scorePreview.cpp scoreInfo.cpp
      logindialog.cpp loginmanager.cpp uploadscoredialog.cpp breaksdialog.cpp searchComboBox.cpp
      help.cpp help.h

      ${OMR_FILES}
      ${AUDIO}
      ${SCRIPT_FILES}
      driver.h
      tremolobarcanvas.h bendcanvas.h fretcanvas.h keycanvas.h harmonycanvas.h
      )

target_link_libraries(mscore
      awl
      mstyle
      diff_match_patch
      bww
      rtf2html
      ${QTSINGLEAPPLICATION_LIBRARIES}
      synthesizer
      midi
      fluid
      effects
      vorbisfile
      libmscore
      xmlstream
      qzip
      beatroot
      kqoauth
      )

if (MINGW)
      set(MSCORE_OUTPUT_NAME ${MUSESCORE_NAME})
endif (MINGW)

#if MSCORE_OUTPUT_NAME is set (e.g, when cmake is called by the user), the output executable will be
#called MSCORE_OUTPUT_NAME instead of 'mscore'. This can be used to have MuseScore stable and unstable
#both installed in the same prefix on a Linux system.
if (MSCORE_OUTPUT_NAME)
  set_target_properties(
      ${ExecutableName}
      PROPERTIES
      OUTPUT_NAME ${MSCORE_OUTPUT_NAME}
      )
endif (MSCORE_OUTPUT_NAME)

if (ZERBERUS)
      target_link_libraries(mscore zerberus synthesizer)
endif ()
if (AEOLUS)
      target_link_libraries(mscore aeolus)
endif ()
if (SOUNDFONT3)
      target_link_libraries(mscore ${VORBIS_LIB} ${OGG_LIB})
endif ()

if (HAS_AUDIOFILE)
      target_link_libraries(mscore audiofile ${SNDFILE_LIB})
endif (HAS_AUDIOFILE)

if (APPLE)
      set_target_properties (mscore
          PROPERTIES
          MACOSX_BUNDLE_INFO_PLIST ${PROJECT_SOURCE_DIR}/build/MacOSXBundleInfo.plist.in)
      #enable dSym generation
      #set_target_properties (mscore
      #   PROPERTIES
      #      XCODE_ATTRIBUTE_DEBUG_INFORMATION_FORMAT "dwarf-with-dsym")
endif(APPLE)

if (OSC)
      target_link_libraries(mscore ofqf)
endif (OSC)

if (MINGW)
   add_custom_command(
      OUTPUT ${PROJECT_BINARY_DIR}/resfile.o
      COMMAND ${QT_WRC_EXECUTABLE} -i mscore.rc -o ${PROJECT_BINARY_DIR}/mscore.res
      COMMAND ${QT_WINE_EXECUTABLE} /home/ws/.wine/drive_c/MingW/bin/windres.exe ${PROJECT_BINARY_DIR}/mscore.res -o ${PROJECT_BINARY_DIR}/resfile.o
      DEPENDS ${PROJECT_SOURCE_DIR}/mscore/data/mscore.rc
      WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/mscore/data
      )
   set_source_files_properties(
      ${PROJECT_BINARY_DIR}/resfile.o
      PROPERTIES generated true
      )
   # Windows: add -mconsole to LINK_FLAGS to get a console window for debug output
   if(CMAKE_BUILD_TYPE MATCHES "DEBUG")
     set_target_properties( mscore
        PROPERTIES
           COMPILE_FLAGS "${PCH_INCLUDE} -g -Wall -Wextra -Winvalid-pch ${QT_DEFINITIONS} -DQT_SVG_LIB -DQT_GUI_LIB -DQT_XML_LIB -DQT_CORE_LIB"
           LINK_FLAGS "${PROJECT_BINARY_DIR}/resfile.o -mwindows -mconsole -L ${CROSSQT}/lib"
        )
   else(CMAKE_BUILD_TYPE MATCHES "DEBUG")
     set_target_properties( mscore
          PROPERTIES
             COMPILE_FLAGS "${PCH_INCLUDE} -Wall -Wextra -Winvalid-pch ${QT_DEFINITIONS} -DQT_SVG_LIB -DQT_GUI_LIB -DQT_XML_LIB -DQT_CORE_LIB"
             LINK_FLAGS "-Wl,-S ${PROJECT_BINARY_DIR}/resfile.o -mwindows -L ${CROSSQT}/lib"
          )
   endif(CMAKE_BUILD_TYPE MATCHES "DEBUG")

   target_link_libraries(mscore
      portaudio
      portmidi
      winmm
      z
      )

   if (OMR)
      target_link_libraries(mscore omr fitz freetype jbig2dec jpeg openjpeg)
      if (OCR)
            target_link_libraries(mscore tesseract_api)
      endif (OCR)
   endif (OMR)

   target_link_libraries(mscore ${QT_mingw_LIBRARIES})

   install( TARGETS mscore RUNTIME DESTINATION bin )

   install( FILES
      ${CROSS}/bin/libgcc_s_dw2-1.dll
      ${CROSS}/bin/libstdc++-6.dll
      ${CROSS}/bin/libwinpthread-1.dll
      ${CROSS}/lib/portaudio.dll
      ${CROSS}/lib/libsndfile-1.dll
      ${CROSS}/lib/libogg.dll
      ${CROSS}/lib/libvorbis.dll
      ${CROSS}/lib/libvorbisfile.dll
      ${CROSS}/opt/bin/ssleay32.dll
      ${CROSS}/opt/bin/libeay32.dll
      ${CROSSQT}/bin/Qt5Core.dll
      ${CROSSQT}/bin/Qt5Widgets.dll
      ${CROSSQT}/bin/Qt5Gui.dll
      ${CROSSQT}/bin/Qt5Xml.dll
      ${CROSSQT}/bin/Qt5Svg.dll
      ${CROSSQT}/bin/Qt5Network.dll
      ${CROSSQT}/bin/Qt5WebChannel.dll
      ${CROSSQT}/bin/Qt5WebKit.dll
      ${CROSSQT}/bin/Qt5WebKitWidgets.dll
      ${CROSSQT}/bin/Qt5XmlPatterns.dll
      ${CROSSQT}/bin/Qt5Qml.dll
      ${CROSSQT}/bin/Qt5Quick.dll
      ${CROSSQT}/bin/Qt5QuickWidgets.dll
      ${CROSSQT}/bin/Qt5Script.dll
      ${CROSSQT}/bin/Qt5Sql.dll
      ${CROSSQT}/bin/Qt5Positioning.dll
      ${CROSSQT}/bin/Qt5PrintSupport.dll
      ${CROSSQT}/bin/Qt5OpenGL.dll
      ${CROSSQT}/bin/Qt5Sensors.dll
      ${CROSSQT}/bin/Qt5Multimedia.dll
      ${CROSSQT}/bin/Qt5MultimediaWidgets.dll
      ${CROSSQT}/bin/icuin53.dll
      ${CROSSQT}/bin/icuuc53.dll
      ${CROSSQT}/bin/icudt53.dll
      ${CROSSQT}/bin/Qt5Help.dll
      ${CROSSQT}/bin/Qt5CLucene.dll
      ${PROJECT_SOURCE_DIR}/build/qt.conf
      DESTINATION bin)

    install(FILES
      ${CROSSQT}/plugins/iconengines/qsvgicon.dll
      DESTINATION bin/iconengines)

    install(FILES
      ${CROSSQT}/plugins/imageformats/qjpeg.dll
      ${CROSSQT}/plugins/imageformats/qmng.dll
      ${CROSSQT}/plugins/imageformats/qsvg.dll
      ${CROSSQT}/plugins/imageformats/qtiff.dll
      DESTINATION bin/imageformats)

    install(FILES
      ${CROSSQT}/plugins/platforms/qwindows.dll
      DESTINATION bin/platforms)

    install(FILES
      ${CROSSQT}/plugins/printsupport/windowsprintersupport.dll
      DESTINATION bin/printsupport)

    install(FILES
      ${CROSSQT}/plugins/sqldrivers/qsqlite.dll
      DESTINATION bin/sqldrivers)

    install(DIRECTORY
      ${CROSSQT}/qml
      DESTINATION .
      REGEX ".*QtWebkit.*" EXCLUDE
      REGEX ".*QtTest.*" EXCLUDE
      REGEX ".*QtSensors.*" EXCLUDE
      REGEX ".*QtGraphicalEffects.*" EXCLUDE
      REGEX ".*QtMultimedia.*" EXCLUDE
      REGEX ".*d\\.dll" EXCLUDE)

else (MINGW)
   target_link_libraries(mscore
      ${ALSA_LIB}
      ${QT_LIBRARIES}
      z
      dl
      pthread
      )
    if (USE_PORTAUDIO)
      target_link_libraries(mscore ${PORTAUDIO_LIB})
    endif (USE_PORTAUDIO)

    if (USE_PULSEAUDIO)
      target_link_libraries(mscore ${PULSEAUDIO_LIBRARY})
    endif (USE_PULSEAUDIO)

   set_target_properties (
      mscore
      PROPERTIES
         COMPILE_FLAGS "${PCH_INCLUDE} -g -Wall -Wno-overloaded-virtual -Winvalid-pch"
      )

   if (OMR)
      target_link_libraries(mscore omr fitz fontconfig freetype jbig2dec jpeg openjpeg)
      if (OCR)
            target_link_libraries(mscore tesseract_api)
      endif (OCR)
   endif (OMR)

   if (APPLE)
     if (USE_PORTMIDI)
       target_link_libraries(mscore portmidi)
     endif (USE_PORTMIDI)
     target_link_libraries(mscore ${OsxFrameworks})
   else (APPLE)
       target_link_libraries(mscore rt)
   endif (APPLE)

   # gold does not use indirect shared libraries for symbol resolution, Linux only
   if (NOT APPLE)
      if(USE_JACK)
         target_link_libraries(mscore dl)
      endif(USE_JACK)
      target_link_libraries(mscore rt)
   endif (NOT APPLE)

   if (APPLE)
      set_target_properties(mscore
        PROPERTIES
           LINK_FLAGS "-stdlib=libc++"
        )
     xcode_pch(mscore all)
     install (TARGETS mscore BUNDLE DESTINATION ${CMAKE_INSTALL_PREFIX})
     install (FILES data/mscore.icns DESTINATION ${Mscore_SHARE_NAME}${Mscore_INSTALL_NAME})
     install (FILES data/musescoreDocument.icns DESTINATION ${Mscore_SHARE_NAME}${Mscore_INSTALL_NAME})
   else (APPLE)
     install( TARGETS mscore RUNTIME DESTINATION bin )
     install( FILES   data/mscore.png DESTINATION share/pixmaps)
   endif (APPLE)
endif (MINGW)

if (APPLE)
     install (FILES
      ../fonts/gootville/Gootville.otf
      ../fonts/gootville/GootvilleText.otf
      ../fonts/mscore/mscore.ttf
      ../fonts/mscore/MScoreText.ttf
      ../fonts/MuseJazz.ttf
      ../fonts/FreeSerif.ttf
      ../fonts/FreeSerifBold.ttf
      ../fonts/FreeSerifItalic.ttf
      ../fonts/FreeSerifBoldItalic.ttf
      ../fonts/FreeSans.ttf
      ../fonts/mscoreTab.ttf
      ../fonts/mscore-BC.ttf
      ../fonts/bravura/Bravura.otf
      ../fonts/bravura/BravuraText.otf
     DESTINATION ${Mscore_SHARE_NAME}${Mscore_INSTALL_NAME}fonts
     )
     install(DIRECTORY
      ${QT_INSTALL_PREFIX}/qml
      DESTINATION ${Mscore_SHARE_NAME}${Mscore_INSTALL_NAME}
      REGEX ".*QtWebkit.*" EXCLUDE
      REGEX ".*QtTest.*" EXCLUDE
      REGEX ".*QtSensors.*" EXCLUDE
      REGEX ".*QtGraphicalEffects.*" EXCLUDE
      REGEX ".*QtMultimedia.*" EXCLUDE
      REGEX ".*QtAudioEngine.*" EXCLUDE
      REGEX ".*_debug\\.dylib" EXCLUDE)
endif (APPLE)

ADD_DEPENDENCIES(${ExecutableName} mops1)
ADD_DEPENDENCIES(${ExecutableName} mops2)

