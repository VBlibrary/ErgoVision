#include <dt-bindings/zmk/matrix_transform.h>

/ {
    chosen {
        zmk,kscan = &default_kscan;
        zmk,matrix_transform = &default_transform;
    };

    default_kscan: kscan {
        compatible = "zmk,kscan-gpio-matrix";
        label = "default_kscan";
        diode-direction = "col2row";

        row-gpios = <
            &pro_micro 1 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 21 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 20 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 19 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 18 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
        >;

        col-gpios = <
            &pro_micro 4 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 5 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 6 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 7 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 8 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
            &pro_micro 9 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)
        >;
    };

    default_transform: matrix_transform {
        compatible = "zmk,matrix-transform";
        columns = <12>;
        rows = <5>;

        map = <
            RC(0,0) RC(0,1) RC(0,2) RC(0,3) RC(0,4) RC(0,5)                RC(0,6) RC(0,7) RC(0,8) RC(0,9) RC(0,10) RC(0,11)
            RC(1,0) RC(1,1) RC(1,2) RC(1,3) RC(1,4) RC(1,5)                RC(1,6) RC(1,7) RC(1,8) RC(1,9) RC(1,10) RC(1,11)
            RC(2,0) RC(2,1) RC(2,2) RC(2,3) RC(2,4) RC(2,5)                RC(2,6) RC(2,7) RC(2,8) RC(2,9) RC(2,10) RC(2,11)
            RC(3,0) RC(3,1) RC(3,2) RC(3,3) RC(3,4) RC(3,5)                RC(3,6) RC(3,7) RC(3,8) RC(3,9) RC(3,10) RC(3,11)                        
                                    RC(4,3) RC(4,4) RC(4,5)                RC(4,6) RC(4,7) RC(4,8)
        >;
    };
};

&pwm0 {
    status = "okay";
};

&led_strip {
    compatible = "sk6812";
    pin = <&pro_micro 
    chain-length = <27>;
    label = "SK6812_Mini_E";
};


&i2c0 {
        status = "disabled";  // Disable I2C by default

        glidepoint0: glidepoint@2a {
            compatible = "cirque,pinnacle";
            reg = <0x2a>;
            status = "disabled";  // Disable Glidepoint by default
            dr-gpios = <&pro_micro 10 (GPIO_ACTIVE_HIGH)>;

            sensitivity = "2x";
            sleep;
            no-taps;
        };
    };

    / {
    /* Assign `input-listener` to all pointing devices */
    /* &glidepoint0 on central, &glidepoint1 on peripheral */
    tpad_central_listener {
        compatible = "zmk,input-listener";
        device = <&glidepoint0>;
        //xy-swap;
        y-invert;
        //x-invert;
        scale-multiplier = <2>;
    };
};

/*
 * Copyright (c) 2020 ZMK Contributors
 *
 * SPDX-License-Identifier: MIT
 */

#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/bt.h>
#include <behaviors/mouse_move.dtsi>
#include <dt-bindings/zmk/mouse.h>

#define ZMK_MOUSE_DEFAULT_MOVE_VAL 1500  // 600
#define ZMK_MOUSE_DEFAULT_SCRL_VAL 20    // 10

#define DEF 0;
#define UPE 1;
#define MSC 2;
/ {
    keymap {
        compatible = "zmk,keymap";

        default_layer {
            bindings = <
                &kp ESC      &kp N1     &kp N2     &kp N3     &kp N4     &kp N5                   &kp N6     &kp N7     &kp N8     &kp N9     &kp N0     &kp DELETE
                &kp CAPSLOCK &kp Q      &kp W      &kp E      &kp R      &kp T                    &kp Y      &kp U      &kp I      &kp O      &kp P      &kp RALT
                &kp LSHIFT   &kp A      &kp S      &kp D      &kp F      &kp G                    &kp H      &kp J      &kp K      &kp L      &kp SEMI   &kp RSHIFT
                &kp LCTRL    &kp Z      &kp X      &kp C      &kp V      &kp B                    &kp N      &kp M      &kp COMMA  &kp DOT    &kp SLASH  &kp RCTRL
                                                   &bt BT_CLR &kp TAB    &kp SPACE                &kp BKSP   &kp ENTER  &bt BT_SEL 1
            >;
        };
    };
};
