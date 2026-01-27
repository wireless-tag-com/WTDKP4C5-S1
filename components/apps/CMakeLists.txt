set(APPS_DIR ./)
file(GLOB_RECURSE APPS_C_SRCS ${APPS_DIR}/*.c)
file(GLOB_RECURSE APPS_CPP_SRCS ${APPS_DIR}/*.cpp)

idf_component_register(
    SRCS ${APPS_C_SRCS} ${APPS_CPP_SRCS}
    INCLUDE_DIRS ${APPS_DIR}
    REQUIRES lvgl__lvgl esp_event esp_wifi nvs_flash esp_driver_jpeg esp_mm esp-brookesia wtdkp4c5_s1_board esp_video pedestrian_detect human_face_detect espressif__esp_lcd_touch_gt911
    EMBED_TXTFILES ${APPS_DIR}/setting/ui/MiSans.ttf)

idf_component_get_property(LVGL_DIR lvgl__lvgl COMPONENT_DIR)
target_include_directories(${COMPONENT_LIB} PRIVATE "${LVGL_DIR}/src/extra/libs/freetype")

target_compile_options(
    ${COMPONENT_LIB}
    PRIVATE
    $<$<COMPILE_LANGUAGE:C>:
        -Wno-format
        -Wno-incompatible-pointer-types
        -Wno-int-conversion
    >
    $<$<COMPILE_LANGUAGE:CXX>:
        -Wno-format
        -Wno-conversion-null
        -Wno-attributes
        -Wno-write-strings
    >
    -DLV_LVGL_H_INCLUDE_SIMPLE
)
